# BillSoft Plus Closing Notes

## In Login Page
* Add Script ```<script src="Scripts/jquery.simplemodal.1.4.4.min.js"></script>```

### Add Css to Login.css 
```css
.Hidden {
    display: none;
}
.popup {
    background-color: #fff;
    padding: 5px;
    border-radius: 5px;
    text-align: center;
}
.ClosePopup {
    position: absolute;
    top: -10px;
    right: -10px;
}
```

 * Add Div Inside form tag  
```html
<div id="DivClosingNote" class="Hidden popup">
<asp:Panel ID="pnlPayorListTitle" runat="server" Style="cursor: move; font-size: medium; font-weight: bold; color: #F7F7F7; background-color: #c12d2d;">
<asp:Label ID="lblPayorListTitle" runat="server" Text="Message" />
</asp:Panel><br />
<asp:Label ID="lblClosingNotes" Font-Bold="true" Style="width: 400px; display: inline-block" runat="server" /><br /><hr />
<asp:Button ID="btnContinue" runat="server" Text="Continue" OnClick="btnContinue_OnClick"/><hr />
</div>

```

## C# Server Side Code (Login)
```cs
DataTable dt = _loginDal.GetClosingNote();
            if (dt.Rows[0]["T_POPUP_FLAG"].ToString() == "Y")
            {
                lblClosingNotes.Text = dt.Rows[0]["T_POPUP_TEXT"].ToString();
                ScriptManager.RegisterStartupScript(this, GetType(), UniqueID, "$('#DivClosingNote').modal({appendTo:'form',opacity: 50, overlayCss: { backgroundColor: '#000' },close:false}); ", true);
            }
            else
                Response.Redirect("~/Menu/M95001.aspx");                             
  ```
  ```cs
    protected void btnContinue_OnClick(object sender, EventArgs e)
        {
            Response.Redirect("~/Menu/M95001.aspx");
        }
  ```
  * Database Login Dal
  ```cs
   public DataTable GetClosingNote()
        {
            return Query($"SELECT T_POPUP_TEXT,T_POPUP_FLAG FROM CLOSING_NOTE");
        }
  ```

  ## For Master Page 
  * add same div *DivClosingNote* inside content template , no need button click event here.
  * add *ClosingNotes()* method inside pageLoad
```
 if (GetParmission())
            {
                ClosingNotes();
                return;
            }
```

  ```cs
  public void ClosingNotes()
        {
            DataTable dt = _commonDal.GetClosingNote();
            if (dt.Rows[0]["T_POPUP_FLAG"].ToString() == "Y")
            {
                lblClosingNotes.Text = dt.Rows[0]["T_POPUP_TEXT"].ToString();
                ScriptManager.RegisterStartupScript(this, GetType(), UniqueID, "$('#DivClosingNote').modal({appendTo:'form',opacity: 50, overlayCss: { backgroundColor: '#000' },close:false}); ", true);
            }
        }
  ```
  * Databse Common Dal
  ```cs
   public DataTable GetClosingNote()
        {
            return Query($"SELECT T_POPUP_TEXT,T_POPUP_FLAG FROM CLOSING_NOTE");           
        }
  ```
