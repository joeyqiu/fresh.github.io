# EUI库--使用EXML

参考地址：http://developer.egret.com/cn/github/egret-docs/extension/EUI/EXML/useEXML/index.html

## 通过加载主题文件

````
private loadTheme() {
        return new Promise((resolve, reject) => {
            // load skin theme configuration file, you can manually modify the file. And replace the default skin.
            //加载皮肤主题配置文件,可以手动修改这个文件。替换默认皮肤。
            let theme = new eui.Theme("resource/default.thm.json", this.stage);
            theme.addEventListener(eui.UIEvent.COMPLETE, () => {
                resolve();
            }, this);

        })
    }
    
在resource/default.thm.json中设定皮肤文件路径

{
	"skins": {
		"eui.Button": "resource/eui_skins/ButtonSkin.exml",
		"eui.CheckBox": "resource/eui_skins/CheckBoxSkin.exml",
		"eui.HScrollBar": "resource/eui_skins/HScrollBarSkin.exml",
		"eui.HSlider": "resource/eui_skins/HSliderSkin.exml",
		"eui.Panel": "resource/eui_skins/PanelSkin.exml",
		"eui.TextInput": "resource/eui_skins/TextInputSkin.exml",
		"eui.ProgressBar": "resource/eui_skins/ProgressBarSkin.exml",
		"eui.RadioButton": "resource/eui_skins/RadioButtonSkin.exml",
		"eui.Scroller": "resource/eui_skins/ScrollerSkin.exml",
		"eui.ToggleSwitch": "resource/eui_skins/ToggleSwitchSkin.exml",
		"eui.VScrollBar": "resource/eui_skins/VScrollBarSkin.exml",
		"eui.VSlider": "resource/eui_skins/VSliderSkin.exml",
		"eui.ItemRenderer": "resource/eui_skins/ItemRendererSkin.exml"
	},
	"autoGenerateExmlsList": true,
	"exmls": [
		"resource/eui_skins/ButtonSkin.exml",
		"resource/eui_skins/CheckBoxSkin.exml",
		"resource/eui_skins/HScrollBarSkin.exml",
		"resource/eui_skins/HSliderSkin.exml",
		"resource/eui_skins/ItemRendererSkin.exml",
		"resource/eui_skins/PanelSkin.exml",
		"resource/eui_skins/ProgressBarSkin.exml",
		"resource/eui_skins/RadioButtonSkin.exml",
		"resource/eui_skins/ScrollerSkin.exml",
		"resource/eui_skins/TextInputSkin.exml",
		"resource/eui_skins/ToggleSwitchSkin.exml",
		"resource/eui_skins/VScrollBarSkin.exml",
		"resource/eui_skins/VSliderSkin.exml"
	],
	"path": "resource/default.thm.json"
}
````

## 直接引用EXML文件

````
var button = new eui.Button();
button.skinName = "resource/skins/ButtonSkin.exml";
this.addChild(button);
````

__皮肤文件推荐放在resource目录下__

## 动态加载EXML文件

````
private init():void{
    EXML.load("skins/ButtonSkin.exml",this.onLoaded,this);
}
private onLoaded(clazz:any,url:string):void{
    var button = new eui.Button();
    button.skinName = clazz;
    this.addChild(button);
}
````

## 嵌入EXML到代码

````javascript
var exmlText = `<?xml version="1.0" encoding="utf-8" ?> 
<e:Skin class="skins.ButtonSkin" states="up,down,disabled" minHeight="50" minWidth="100" xmlns:e="http://ns.egret.com/eui"> 
    <e:Image width="100%" height="100%" scale9Grid="1,3,8,8" alpha.disabled="0.5"
             source="button_up_png"
             source.down="button_down_png"/> 
    <e:Label id="labelDisplay" top="8" bottom="8" left="8" right="8"
             size="20" fontFamily="Tahoma 'Microsoft Yahei'"
             verticalAlign="middle" textAlign="center" text="按钮" textColor="0x000000"/> 
    <e:Image id="iconDisplay" horizontalCenter="0" verticalCenter="0"/> 
</e:Skin>`;
var button = new eui.Button();
button.skinName = exmlText;
this.addChild(button);
````