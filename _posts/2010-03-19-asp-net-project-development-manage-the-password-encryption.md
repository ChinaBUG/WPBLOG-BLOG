---
ID: 358
post_title: >
  ASP.NET项目懒人开发-后台管理之密码加密
author: ChinaBUG
post_excerpt: |
  　　典型的ASP应用，后台可以添加管理员，修改管理员密码，再添加时还能顺便对管理员密码进行MD5加密。
  　　现在在.NET中，使用GridView来显示管理员列表，修改管理员密码，怎么加密呢？因为数据库更新操作都是自动的，我们怎么能够在更新的时候把密码加密并存进数据库呢？看下文吧，这是我的解决办法。
layout: post
permalink: >
  http://blog.ipodmp.com/2010/03/asp-net-project-development-manage-the-password-encryption.html
published: true
post_date: 2010-03-19 09:25:06
---
　　典型的ASP应用，后台可以添加管理员，修改管理员密码，再添加时还能顺便对管理员密码进行MD5加密。

　　现在在.NET中，使用GridView来显示管理员列表，修改管理员密码，怎么加密呢？因为数据库更新操作都是自动的，我们怎么能够在更新的时候把密码加密并存进数据库呢？看下文吧，这是我的解决办法。

　　PS:我是初学者，所以我不懂得有什么高深的办法可以做到这一步，恩，还好我解决啦，你有其他办法可以与我交流噢。

　　整个功能的实现需要一个表，一个文件。

　　直接提出代码，大家自己看噢，有问题再问噢。

　　数据库结构如下：

WS_Admin
 admin_id | admin_username | admin_password
1                   |admin                         | 21232F297A57A5A743894A0E4A801FC3

admin_password.aspx内代码如下：
<h6>                    &lt;asp:DetailsView ID="DetailsView1" runat="server" AutoGenerateRows="False"
                        DataKeyNames="admin_id" DataSourceID="ADS_Show_insert" GridLines="None"
                        Width="300px" EnableModelValidation="True" OnItemInserting="MyDetailsView_ItemInserting"
                        &gt;
                        &lt;Fields&gt;
                            &lt;asp:TemplateField&gt;
                                &lt;ItemTemplate&gt;
                                    &lt;asp:LinkButton ID="LinkButton2" runat="server" CausesValidation="False" CommandName="New" Text="添加新的管理员"&gt;&lt;/asp:LinkButton&gt;
                                &lt;/ItemTemplate&gt;
                                &lt;InsertItemTemplate&gt;
                                   管理员账号：&lt;asp:TextBox ID="TextBox1" runat="server" Text='&lt;%# Bind("admin_username") %&gt;'&gt;&lt;/asp:TextBox&gt;&lt;br/&gt;
                                    管理员密码：&lt;asp:TextBox ID="TextBox2" runat="server" Text='&lt;%# Bind("admin_password") %&gt;'&gt;&lt;/asp:TextBox&gt;&lt;br/&gt;
                                   &lt;asp:LinkButton ID="LinkButton1" runat="server" CausesValidation="True" CommandName="Insert" Text="插入"&gt;&lt;/asp:LinkButton&gt;
                                                &lt;asp:LinkButton ID="LinkButton3" runat="server" CausesValidation="False" CommandName="Cancel" Text="取消"&gt;&lt;/asp:LinkButton&gt;
                                &lt;/InsertItemTemplate&gt;
                            &lt;/asp:TemplateField&gt;
                        &lt;/Fields&gt;
                    &lt;/asp:DetailsView&gt;
                    &lt;asp:Label ID="Lbl_error" runat="server" Font-Bold="True" ForeColor="Red"&gt;&lt;/asp:Label&gt;
                    &lt;asp:AccessDataSource ID="ADS_Show_insert" runat="server"
                        ConflictDetection="CompareAllValues" DataFile="~/App_Data/NewSunPaper2010.mdb"
                        InsertCommand="INSERT INTO [WS_Admin] ([admin_username], [admin_password]) VALUES (?, ?)"
                        OldValuesParameterFormatString="original_{0}"
                        SelectCommand="SELECT * FROM [WS_Admin]"
                        &gt;
                        &lt;InsertParameters&gt;
                            &lt;asp:Parameter Name="admin_username" Type="String" /&gt;
                            &lt;asp:Parameter Name="admin_password" Type="String" /&gt;
                        &lt;/InsertParameters&gt;
                    &lt;/asp:AccessDataSource&gt;
                    &lt;asp:GridView ID="GridView1" runat="server" AllowPaging="True"
                        AutoGenerateColumns="False" BackColor="White" BorderColor="#CCCCCC"
                        BorderWidth="1px" CellPadding="4" DataKeyNames="admin_id"
                        DataSourceID="ADS_Show_Config" ForeColor="Black" GridLines="Horizontal"
                        PageSize="15" BorderStyle="None" Width="700px"
                        OnRowUpdating="MyGridView_RowUpdating" EnableModelValidation="True"
                        &gt;
                        &lt;Columns&gt;
                            &lt;asp:TemplateField HeaderText="账号" SortExpression="admin_username"&gt;
                                &lt;EditItemTemplate&gt;
                                    &lt;asp:Label ID="Label1" runat="server" Text='&lt;%# Eval("admin_username") %&gt;'&gt;&lt;/asp:Label&gt;
                                &lt;/EditItemTemplate&gt;
                                &lt;ItemTemplate&gt;
                                    &lt;asp:Label ID="Label2" runat="server" Text='&lt;%# Bind("admin_username") %&gt;'&gt;&lt;/asp:Label&gt;
                                &lt;/ItemTemplate&gt;
                                &lt;ItemStyle HorizontalAlign="Center" Width="150px" /&gt;
                            &lt;/asp:TemplateField&gt;
                            &lt;asp:TemplateField HeaderText="密码" SortExpression="admin_password"&gt;
                                &lt;EditItemTemplate&gt;
                                    &lt;asp:TextBox ID="TextBox1" runat="server" Text='&lt;%# Bind("admin_password") %&gt;'&gt;&lt;/asp:TextBox&gt;
                                &lt;/EditItemTemplate&gt;
                                &lt;ItemTemplate&gt;
                                    &lt;asp:Label ID="Label1" runat="server" Text='&lt;%# Bind("admin_password") %&gt;'&gt;&lt;/asp:Label&gt;
                                &lt;/ItemTemplate&gt;
                                &lt;ControlStyle Width="250px" /&gt;
                                &lt;ItemStyle Width="200px" /&gt;
                            &lt;/asp:TemplateField&gt;
                            &lt;asp:TemplateField ShowHeader="False"&gt;
                                &lt;EditItemTemplate&gt;
                                    &lt;asp:LinkButton ID="LinkButton1" runat="server" CausesValidation="True"
                                        CommandName="Update" Text="更新"&gt;&lt;/asp:LinkButton&gt;
                                    &amp;nbsp;&lt;asp:LinkButton ID="LinkButton2" runat="server" CausesValidation="False"
                                        CommandName="Cancel" Text="取消"&gt;&lt;/asp:LinkButton&gt;
                                &lt;/EditItemTemplate&gt;
                                &lt;ItemTemplate&gt;
                                    &lt;asp:LinkButton ID="LinkButton1" runat="server" CausesValidation="False"
                                        CommandName="Edit" Text="修改"&gt;&lt;/asp:LinkButton&gt;
                                    &amp;nbsp;&lt;asp:LinkButton ID="LinkButton2" runat="server" CausesValidation="False"
                                        CommandName="Delete" Text="删除"&gt;&lt;/asp:LinkButton&gt;
                                &lt;/ItemTemplate&gt;
                                &lt;ItemStyle Width="100px" /&gt;
                            &lt;/asp:TemplateField&gt;
                        &lt;/Columns&gt;
                    &lt;/asp:GridView&gt;
                    &lt;asp:AccessDataSource ID="ADS_Show_Config" runat="server"
                        DataFile="~/App_Data/NewSunPaper.mdb"
                        OldValuesParameterFormatString="original_{0}"
                        SelectCommand="SELECT * FROM [WS_Admin] order by admin_id desc"
                        ConflictDetection="CompareAllValues" 
                        InsertCommand="INSERT INTO [WS_Admin] ([admin_username], [admin_password]) VALUES (?, ?)"
                        UpdateCommand="UPDATE [WS_Admin] SET [admin_password] = ? WHERE [admin_id] = ?"
                        &gt;
                        &lt;InsertParameters&gt;
                            &lt;asp:Parameter Name="admin_id" Type="Int32" /&gt;
                            &lt;asp:Parameter Name="admin_username" Type="String" /&gt;
                            &lt;asp:Parameter Name="admin_password" Type="String" /&gt;
                        &lt;/InsertParameters&gt;
                        &lt;SelectParameters&gt;
                            &lt;asp:SessionParameter Name="admin_username" SessionField="admin_username" Type="String" /&gt;
                        &lt;/SelectParameters&gt;
                        &lt;UpdateParameters&gt;
                            &lt;asp:Parameter Name="admin_password" Type="String" /&gt;
                            &lt;asp:Parameter Name="original_admin_id" Type="Int32" /&gt;
                        &lt;/UpdateParameters&gt;
                    &lt;/asp:AccessDataSource&gt;</h6>
admin_password.aspx.vb内代码如下：
<h6>    Protected Sub <span style="color: #ff0000;">MyGridView_RowUpdating</span>(ByVal sender As Object, ByVal e As System.Web.UI.WebControls.GridViewUpdateEventArgs)
        e.NewValues(0) = FormsAuthentication.HashPasswordForStoringInConfigFile(e.NewValues(0), "MD5")
    End Sub</h6>
<h6>    Protected Sub <span style="color: #ff0000;">MyDetailsView_ItemInserting</span>(ByVal sender As Object, ByVal e As System.Web.UI.WebControls.DetailsViewInsertEventArgs)
        '判断是否存在相同的管理员账号
        Dim MySQL As String
        MySQL = "SELECT * FROM [WS_Admin] WHERE admin_username='" &amp; e.Values(0) &amp; "'"
        Dim strConn As String = "Provider=Microsoft.Jet.OLEDB.4.0;Data Source=" &amp; Server.MapPath("~/App_Data/NewSunPaper.mdb") &amp; ";"
        Dim MyConn As New OleDbConnection(strConn)
        Dim Cmd As New OleDbCommand(MySQL, MyConn)
        MyConn.Open()
        Dim objDR As OleDbDataReader
        objDR = Cmd.ExecuteReader(System.Data.CommandBehavior.CloseConnection)
        If objDR.Read() Then
            Lbl_error.Text = "对不起，该用户名已经存在."
            e.Cancel = True
        Else
            e.Values(1) = FormsAuthentication.HashPasswordForStoringInConfigFile(e.Values(1), "MD5")
            Lbl_error.Text = ""
        End If
        objDR.Close()
        MyConn.Close()
    End Sub</h6>