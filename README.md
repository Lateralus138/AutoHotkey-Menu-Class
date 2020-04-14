# AutoHotkey _Menu Class

<div style="padding: 0;">
<img src="./images/AutoHotkey-Class.png" width="186" /><br>
<img src="./images/AutoHotkey-Scripting.png" width="218" /><br>
<img src="./images/AutoHotkey-Coding.png" width="203" /><br>
<img src="./images/AutoHotkey-Programming.png" width="257" /><br>
<img src="./images/AutoHotkey-Is-The-Best.png" width="237" /><br>
</div>

Create objects of system tray/context menus in the AutoHotkey scripting language&#46;

This is a full-featured class that has methods to do anything the Menu commands can already do, but it creates objects of information to make working with the menus cleaner &amp; more organzied&#46;
<div style="width:400px;height:150px;overflow:scroll;padding:5px;">
<pre>
Class _Menu
{
    __New(name:="Tray",first_item:="",first_label:="",first_options:="",iconv*)
    {
        this.ColorList:=    {   Black:"0x000000" ,Silver:"0xC0C0C0" ,Gray:"0x808080" ,White:"0xFFFFFF" 
                            ,   Maroon:"0x800000" ,Red:"0xFF0000",Purple:"0x800080" ,Fuchsia:"0xFF00FF" 
                            ,   Green:"0x008000" ,Lime:"0x00FF00" ,Olive:"0x808000" ,Yellow:"0xFFFF00" 
                            ,   Navy:"0x000080" ,Blue:"0x0000FF" ,Teal:"0x008080" ,Aqua:"0x00FFFF"  }
        this.ClickCount    :=  2
        this.MenuName:=name
        if (iconv.MaxIndex())
        {   
            this.Icon(((this.MenuName!="Tray")?this.MenuName:"") ,iconv[1],iconv[2],iconv[3])
        }
        this.Add(first_item,((first_item)?first_label:""),((first_item)?first_options:""))
    }
    Add(item_name:="",label_or_menu:="",options*)
    {   
        if (! item_name)
        {
            this.MenuFunc_Small(A_ThisFunc)
            return
        }
        this.MenuItems[item_name]:= {   name:item_name
                                    ,   label:label_or_menu
                                    ,   options:this.ArrayToString(options)}
        Menu,   % this.MenuName
            ,   % this.ThisFuncName(A_ThisFunc)
            ,   % this.MenuItems[item_name].name
            ,   % this.MenuItems[item_name].label
            ,   % this.MenuItems[item_name].options
    }
    Insert(existing_item,new_item,label_or_menu,options*)
    {   
        if (this.MenuItems[existing_item].name)
        {
            this.MenuItems[new_item]:= {   name:new_item
                                            ,   label:label_or_menu
                                            ,   options:this.ArrayToString(options)}
            Menu    ,   % this.MenuName
                    ,   % this.ThisFuncName(A_ThisFunc)
                    ,   % this.MenuItems[existing_item].name
                    ,   % this.MenuItems[new_item].name
                    ,   % this.MenuItems[new_item].label
                    ,   % this.MenuItems[new_item].options         
        }

    }
    Delete(item_name)
    {
        if (this.MenuItems[item_name].name)
        {
            Menu    ,   % this.MenuName
                    ,   % this.ThisFuncName(A_ThisFunc)
                    ,   % this.MenuItems[item_name].name
            this.MenuItems[item_name]:=""
        }
    }
    DeleteAll()
    {
        Menu    ,   % this.MenuName
                ,   % this.ThisFuncName(A_ThisFunc)
    }
    Rename(item_name,new_name)
    {
        if (this.MenuItems[item_name].name)
        {
            Menu    ,   % this.MenuName
                    ,   % this.ThisFuncName(A_ThisFunc)
                    ,   % this.MenuItems[item_name].name
                    ,   % new_name
            this.MenuItems[new_name]:=this.MenuItems[item_name]
            this.MenuItems[new_name].name:=new_name
            this.MenuItems[item_name]:=""
            return this.MenuItems[item_name].name
        }
    }
    Check(item_name)
    {
        if (this.MenuItems[item_name].name)
        {
            this.MenuItems[item_name].Checked:=True
        }
        this.MenuFunc_Big(item_name,A_ThisFunc)
        return this.MenuItems[item_name].Checked
    }
    Uncheck(item_name)
    {
        if (this.MenuItems[item_name].name)
        {
            this.MenuItems[item_name].Checked:=False
        }
        this.MenuFunc_Big(item_name,A_ThisFunc)
        return this.MenuItems[item_name].Checked
    }
    ToggleCheck(item_name)
    {
        if (this.MenuItems[item_name].name)
        {
            this.MenuItems[item_name].Checked:=(!this.MenuItems[item_name].Checked)
        }
        this.MenuFunc_Big(item_name,A_ThisFunc)
        return this.MenuItems[item_name].Checked
    }
    Enable(item_name)
    {
        if (this.MenuItems[item_name].name)
        {
            this.MenuItems[item_name].Enabled:=True
        }
        this.MenuFunc_Big(item_name,A_ThisFunc)
        return this.MenuItems[item_name].Enabled
    }
    Disable(item_name)
    {
        if (this.MenuItems[item_name].name)
        {
            this.MenuItems[item_name].Enabled:=False
        }
        this.MenuFunc_Big(item_name,A_ThisFunc)
        return this.MenuItems[item_name].Enabled
    }
    ToggleEnable(item_name)
    {
        if (this.MenuItems[item_name].name)
        {
            this.MenuItems[item_name].Enabled:=(!this.MenuItems[item_name].Enabled)
        }
        this.MenuFunc_Big(item_name,A_ThisFunc)
        return this.MenuItems[item_name].Enabled
    }
    Default(item_name:="")
    {
            Menu    ,   % this.MenuName
                    ,   % this.ThisFuncName(A_ThisFunc)
                    ,   % this.MenuItems[item_name].name
    }
    NoDefault()
    {
        this.MenuFunc_Small(A_ThisFunc)
    }
    Standard()
    {
        this.MenuFunc_Small(A_ThisFunc)
    }
    NoStandard()
    {
        this.MenuFunc_Small(A_ThisFunc)
    }
    Icon(menu_item:="",iconv*)
    {   
        if (FileExist(iconv[1]))
        {   
            if (menu_item)
            {   
                if (! this.MenuItems[menu_item].name)
                {
                    return
                }
                this.MenuItems[menu_item].IconObject:={}
                this.MenuItems[menu_item].IconObject.file:=iconv[1]
                if iconv[2] is integer
                {
                    this.MenuItems[menu_item].IconObject.number:=iconv[2]    
                }
                if iconv[3] is integer
                {
                    this.MenuItems[menu_item].IconObject.width:=iconv[3]    
                }
                Menu    ,   % this.MenuName
                        ,   % this.ThisFuncName(A_ThisFunc)
                        ,   % this.MenuItems[menu_item].name
                        ,   % this.MenuItems[menu_item].IconObject.file
                        ,   % this.MenuItems[menu_item].IconObject.number
                        ,   % this.MenuItems[menu_item].IconObject.width
                return this.MenuItems[menu_item].IconObject
            }
            this.IconObject:={}
            this.IconObject   :=    {   file:iconv[1]
                                    ,   number:iconv[2]
                                    ,   freeze:iconv[3]}
            Menu    ,   % this.MenuName
                    ,   % this.ThisFuncName(A_ThisFunc)
                    ,   % this.IconObject.file
                    ,   % this.IconObject.number
                    ,   % this.IconObject.freeze
            return this.IconObject
        }     
    }
    NoIcon(menu_item:="")
    {
        if (menu_item)
        {
            if (! this.MenuItems[menu_item].name)
            {
                return
            }
            Menu    ,   % this.MenuName
                    ,   % this.ThisFuncName(A_ThisFunc)
                    ,   % this.MenuItems[menu_item].name
            this.MenuItems[menu_item].IconObject:=""
            return
        }
        this.MenuFunc_Small(A_ThisFunc)
        this.IconObject:=""
    }
    Tip(string:="")
    {
        if (string)
        {
            this.ToolTip:=string
            Menu    ,   % this.MenuName
                    ,   % this.ThisFuncName(A_ThisFunc)
                    ,   % this.ToolTip
            return this.ToolTip
        }
        this.ToolTip:=""
        this.MenuFunc_Small(A_ThisFunc)
    }
    Show(x:="",y:="",coord_mode:="Relative")
    {
        this.Position:={x:x,y:y,mode:coord_mode}
        CoordMode,Menu,% this.Position.mode
        Menu,% this.MenuName,% this.ThisFuncName(A_ThisFunc),% this.Position.x,% this.Position.y
        CoordMode,Menu,Relative
        return this.Position
    }
    Color(color:="Default",single:=False)
    {
        single:=(single?"Single":"")
        if ((color="Default" or) Or (color:=""))
        {
            this.ColorObject:=""
        }
        else if color is not xdigit
        {
            if (! this.ColorList[color])
            {
                return
            }
            this.ColorObject:=  {   color:this.ColorList[color]
                                ,   level:single}
        }
        else
        {
            this.ColorObject:=  {   color:color
                                ,   level:single}
        }
        Menu    ,   % this.MenuName
                ,   % this.ThisFuncName(A_ThisFunc)
                ,   % this.ColorObject.color
                ,   % this.ColorObject.level
        return this.ColorObject
    }
    Click(count:=2)
    {
        if count is not integer
        {
            return
        }
        if (this.MenuName!="Tray")
        {
            return
        }
        this.ClickCount:=count
        Menu    ,   % this.MenuName
                ,   % this.ThisFuncName(A_ThisFunc)
                ,   % this.ClickCount
        return this.ClickCount
    }
    MainWindow()
    {
        if (this.MenuName="Tray")
        {
            this.Main:=True
            this.MenuFunc_Small(A_ThisFunc)
            return this.Main
        }
    }
    NoMainWindow()
    {
        if (this.MenuName="Tray")
        {
            this.Main:=False
            this.MenuFunc_Small(A_ThisFunc)
            return this.Main
        }
    }
    UseErrorLevel(state:="Off")
    {
        this.UseError:=((state="Off")?"Off":"On")
        Menu    ,   % this.MenuName
                ,   % this.ThisFuncName(A_ThisFunc)
                ,   % this.UseError
        return this.UseError            
    }
    Separator(optional_string:="")
    {
        Menu    ,   % this.MenuName
                ,   Add
                ,   %optional_string%
                ,   % ((optional_string)?this.DummyFunc():"")
    }
    ArrayToString(array)
    {
        if (array.MaxIndex())
        {
            for index, item in array
            {
                string.=item A_Space
            }
        }
        return (string?string:array)
    }
    ThisFuncName(full_func_name)
    {
        fn:=StrSplit(full_func_name,".")
        return fn[fn.MaxIndex()]
    }
    MenuFunc_Big(item_name,func)
    {
        if (this.MenuItems[item_name].name)
        {
            Menu    ,   % this.MenuName
                    ,   % this.ThisFuncName(func)
                    ,   % this.MenuItems[item_name].name
        }
    }
    MenuFunc_Small(func)
    {
        Menu    ,   % this.MenuName
                ,   % this.ThisFuncName(func)
    }
    DummyFunc()
    {
        return A_ThisFunc
    }
}
</pre>
</div>