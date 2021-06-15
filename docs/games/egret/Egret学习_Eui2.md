# EUI库--自定义皮肤



通过EUI库，在皮肤中把游戏元素给先创建出来，能够节省很多代码量，避免动态创建。

````xml
<?xml version="1.0" encoding="utf-8"?>
<e:Skin class="FlyBirdSkin" width="750" height="1334" xmlns:e="http://ns.egret.com/eui">
  <e:Image source="bg_jpg" width="100%" height="100%" />
  <e:Image id="upPipe1" source="pipe_up_png" touchEnabled="false" x="800" y="0" />
  <e:Image id="upPipe2" source="pipe_up_png" touchEnabled="false" x="1600" y="0" />
  <e:Image id="downPipe1" source="pipe_down_png" touchEnabled="false" x="800" y="0" />
  <e:Image id="downPipe2" source="pipe_down_png" touchEnabled="false" x="1600" y="0" />
  <e:Group width="100%" height="100%" id="startGroup">
    <e:Button label="Start" id="startBtn" verticalCenter="0" horizontalCenter="0"/>
  </e:Group>
  <e:Group width="100%" height="100%" id="reStartGroup" visible="false">
    <e:Button label="GameOver" id="reStartBtn" verticalCenter="0" horizontalCenter="0"/>
  </e:Group>
  <e:Image id="floor" source="floor_jpg" touchEnabled="false" bottom="0" x="0"/>
</e:Skin>
````



上述skin中，有6个Image元素，2个Group元素，和2个Button元素，如果用上述办法，可以减少每个元素的创建，在egret中，创建一个image的代码量如下：

````javascript
private showFloor(){
    let floor = Tool.createBitmapByName('floor_jpg');
    floor.width = 1280;
    floor.height = 107;
    floor.y = this.stage.stageHeight - 107;
    floor.x = 0;
    this.stage.addChild(floor);
    this.floor = floor;
    Floor.y = floor.y;
  }
````



## 自定义皮肤使用

在default.thm.json文件中声明

```json
{
	"skins": {
		"MyButton": "resource/MyButtonSkin.exml",
		"eui.Button": "resource/eui_skins/ButtonSkin.exml"
	},
	"autoGenerateExmlsList": true,
	"exmls": [
		"resource/FlyBirdSkin.exml",
		"resource/MyButtonSkin.exml"
	],
	"path": "resource/default.thm.json"
}
```

然后在main.ts中加载上述主题，这段代码是默认自带的:



```javascript
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
```

