This issue affects both the file poperties dialog's font page and an in development dialog (see [[gh5571-Writer->Format->Sections->Options is broken]])

It manifests as the following:
`JSDialogBuilder: Unsupported control type: "dockingwindow"`

(as well as aroung 1000 lines of json when using protocol logging)

This json does not seem to be associated with the action taken #wtf
Adding a line to `b/s/c/Control.JSDialogBuilder.js` to handle it as any other dialog does not fix the issue, #reverted 

a critical part of the puzzle it that both places where this occurs have a nonstandard way of adding a tab page
![[Pasted image 20230219184310.png]]
![[Pasted image 20230219184323.png]]

tab pages are much more normally added with
![[Pasted image 20230219184351.png]]
which is a simple reference to the create function and then a nullptr

the addfonttabpage is more different as idk wtf is going on there

changing it to
![[Pasted image 20230219184713.png]]
as one might assume would fix it simply removes the tab page
![[Pasted image 20230219184741.png]]

- [x] 📅 2023-02-20 `/source/dialog/tabdlg.cxx` probably useful ✅ 2023-02-19
- [x] 📅 2023-02-20 go look at `RID_SVXPAGE_BKG` ✅ 2023-02-20

AddTabPage is polymorphic, it can take:
- tab name, create function, "ranges" function
- tab name, create function *id* (create and ranges functions get fetched with `GetTabPageCreatorFunc` and `GetTabPageRangesFunc` then calls the first type)
- tab name, *rider text??*, create function (does some stuff then calls the first type with a nullptr as the ranges function)
- tab name, *rider text??*, create function *id* (does some stuff then calls the second type)

or, TL;DR we may be able to state that tab page always calls this first type of tab page addition
```cpp
// Reformatted for your convenience, dear reader
void SfxTabDialogController::AddTabPage(
	const OString &rName /* Page ID */,
    CreateTabPage pCreateFunc  /* Pointer to the Factory Method */,
    GetTabPageRanges pRangesFunc /* Pointer to the Method for querying Ranges onDemand */
) {
    m_pImpl->aData.push_back(new Data_Impl(rName, pCreateFunc, pRangesFunc));
}
```

the "normal" pages just call this variation without the `pRangesFunc`

the broken pages are more interesting:
- the background page calls `pFact->GetTabPageCreatorFunc(RID_SVXPAGE_BKG)` to get the creator function
- the font tab page calls the third variation, presumably therefore `SfxResId` returns a string which is our "rider text"

I am curious as to *why* the background page does not call the second variation, let's test it (presumably there's no ranges function?)
The same thing happens, honestly that only makes me more curious

`RID_SVXPAGE_BKG` is used a hell of a lot:
```bash
{19:51}~/Code/c-cpp/libreoffice/docker-tree:0110c0b6fadb ✗ ➭ rg RID_SVXPAGE_BKG                      
include/svx/dialogs.hrc
43:#define RID_SVXPAGE_BKG                     (RID_SVX_START +  57)

cui/source/dialogs/sdrcelldlg.cxx
49:        AddTabPage("highlight", RID_SVXPAGE_BKG);

cui/source/factory/dlgfact.cxx
1261:        case RID_SVXPAGE_BKG :
1372:        case RID_SVXPAGE_BKG:

sd/uiconfig/sdraw/ui/drawchardialog.ui
276:              <object class="GtkLabel" id="RID_SVXPAGE_BKG">
279:                <property name="label" translatable="yes" context="drawchardialog|RID_SVXPAGE_BKG">Highlighting</property>

sd/uiconfig/sdraw/ui/drawprtldialog.ui
854:              <object class="GtkLabel" id="RID_SVXPAGE_BKG">
857:                <property name="label" translatable="yes" context="drawprtldialog|RID_SVXPAGE_BKG">Highlighting</property>

sd/source/ui/dlg/dlgchar.cxx
44:    AddTabPage("RID_SVXPAGE_BKG", pFact->GetTabPageCreatorFunc(RID_SVXPAGE_BKG), nullptr);
63:    else if (rId == "RID_SVXPAGE_BKG")

sd/source/ui/dlg/prltempl.cxx
142:    AddTabPage( "RID_SVXPAGE_BKG", pFact->GetTabPageCreatorFunc( RID_SVXPAGE_BKG ), nullptr);
164:        RemoveTabPage( "RID_SVXPAGE_BKG" );
264:    else if (rId == "RID_SVXPAGE_BKG")

sd/source/ui/dlg/tabtempl.cxx
64:    AddTabPage("background", RID_SVXPAGE_BKG);

reportdesign/source/ui/dlg/dlgpage.cxx
43:        AddTabPage("background", pFact->GetTabPageCreatorFunc( RID_SVXPAGE_BKG ), nullptr );
48:        AddTabPage("background", pFact->GetTabPageCreatorFunc( RID_SVXPAGE_BKG ), nullptr );
56:        AddTabPage("background", pFact->GetTabPageCreatorFunc( RID_SVXPAGE_BKG ), nullptr );

sc/source/ui/styleui/styledlg.cxx
57:        AddTabPage("background", pFact->GetTabPageCreatorFunc( RID_SVXPAGE_BKG ), pFact->GetTabPageRangesFunc( RID_SVXPAGE_BKG ) );
75:        AddTabPage("background", pFact->GetTabPageCreatorFunc( RID_SVXPAGE_BKG ), pFact->GetTabPageRangesFunc( RID_SVXPAGE_BKG ));

sc/source/ui/attrdlg/attrdlg.cxx
58:    OSL_ENSURE(pFact->GetTabPageCreatorFunc( RID_SVXPAGE_BKG ), "GetTabPageCreatorFunc fail!");
59:    AddTabPage( "background",  pFact->GetTabPageCreatorFunc( RID_SVXPAGE_BKG ), nullptr );

sc/source/ui/miscdlgs/textdlgs.cxx
42:        AddTabPage("background", RID_SVXPAGE_BKG);

sw/source/uibase/app/appopt.cxx
503:            ::CreateTabPage fnCreatePage = pFact->GetTabPageCreatorFunc( RID_SVXPAGE_BKG );

sw/source/ui/table/tabledlg.cxx
1226:    AddTabPage("background", pFact->GetTabPageCreatorFunc(RID_SVXPAGE_BKG), nullptr);

sw/source/ui/fmtui/tmpdlg.cxx
103:            AddTabPage("background", pFact->GetTabPageCreatorFunc( RID_SVXPAGE_BKG ), pFact->GetTabPageRangesFunc( RID_SVXPAGE_BKG ));
128:            AddTabPage("highlighting", pFact->GetTabPageCreatorFunc( RID_SVXPAGE_BKG ), pFact->GetTabPageRangesFunc( RID_SVXPAGE_BKG ));
325:    // for the SvxBrushItem (see RID_SVXPAGE_BKG)

sw/source/ui/frmdlg/pattern.cxx
32:    ::CreateTabPage fnCreatePage = pFact->GetTabPageCreatorFunc(RID_SVXPAGE_BKG);

sw/source/ui/dialog/uiregionsw.cxx
1375:    AddTabPage("background", pFact->GetTabPageCreatorFunc(RID_SVXPAGE_BKG), nullptr);
2023:    AddTabPage("background", RID_SVXPAGE_BKG);

sw/source/ui/chrdlg/chardlg.cxx
77:    AddTabPage("background", pFact->GetTabPageCreatorFunc( RID_SVXPAGE_BKG ), nullptr );

sw/source/ui/index/cnttab.cxx
279:    AddTabPage("background", pFact->GetTabPageCreatorFunc(RID_SVXPAGE_BKG), nullptr);
```

the reference in `cui/source/factory/dlgfact.cxx` looks important
```cpp
// GetTabPageCreatorFunc
        case RID_SVXPAGE_BKG:
            return SvxBkgTabPage::Create;

```
```cpp
// GetTabPageRangesFunc
        case RID_SVXPAGE_BKG:
            return SvxBkgTabPage::GetRanges;
```

but it's actually very boring. It's interesting that there's an unused ranges function though

I considered, for completeness, using this value directly not the call but felt it poor form to simply include
This does give us a clue though: this is a Svx tab page, everything else is Sw. Maybe they are written differently enough to break everything? That would suggest a very different bug to the font page though and the symptoms manifest similarly

