###Info
As many of you know it's almost impossible to play Ori with non-XBOX gamepad: all buttons are messed up and Unity's built-in remapper doesn't work properly (you can't rebind jump button). This tool is intended to fix it.

###How it works
The included PowerShell script will replace a part of the Ori's code contained in the `Assembly-CSharp.dll` file. This new code will query the Unity engine for the remapped buttons, instead of original ones, requested by game.

###Technical details
The modfied code is in the `MoonInput.GetButton(string buttonName)` method in the `Assembly-CSharp.dll`.

Original:
```csharp
public static bool GetButton(string buttonName)
{
    return Input.GetButton(buttonName);
}
```

Patched:
```csharp
public static bool GetButton(string buttonName)
{
    // array of the remapped button's numbers, generated by PS1 script from ini file
    string[] strArray = new string[] { "1", "2", "0", "3", "4", "5", "12", "9", "10", "11", "6", "7" }; 
    Match match = Regex.Match(buttonName, @"^(Joystick\d{1}Button)(\d{1})$", RegexOptions.Singleline);
    if (match.Success)
    {
        return Input.GetButton(match.Groups[1].Value + strArray[int.Parse(match.Groups[2].Value)]);
    }
    return Input.GetButton(buttonName);
}
```

###How-to
 1. Download [JoystickTest application]
 2. Go to https://github.com/beatcracker/Ori-Controller-Remap
 2. In the bottom right corner click `Download ZIP`
 3. Unpack downloaded ZIP file
 4. Go to the `Ori-Controller-Remap-master` folder
 5. Open `Ori_Controller_Remap.ini` file in Notepad. This file contains controller button mappings. I've included one for my gamepad (`Cyborg Rumble Pad`) and one for you to edit (`Your Controller Name`).
 6. Run JoystickTest application
 7. Punch buttons on your controller, note their numbers in JoystickTest, edit ini file accordingly
 8. Replace `Your Controller Name` with any text you like (your controller name, your pet name, your maiden name - it's all up to you).
 9. Save `Ori_Controller_Remap.ini` file
 10. Double-click `Ori_Controller_Remap.cmd` file, it will launch PowerShell script
 11. When asked, select configuration you've edited earlier in the ini file
 12. Press any key and wait for script to patch your Ori with new controller mapping
 
###Troubleshooting

**Q:** Script can't find my Ori installation (`Assembly-CSharp.dll` file)

**A:** Are you running non-Steam version of Ori? Script uses registry keys created by Steam to locate Ori installation folder. If it can't find it, it looks for the required files in the script folder. So you have to copy those files in the script folder:

 * Assembly-CSharp.dll
 * mscorlib.dll
 * System.dll
 * UnityEngine.dll

Those files are located in the `X:\Ori_Installation_Directory\ori_Data\Managed\`. After the patch, you have to copy and replace `Assembly-CSharp.dll` file back to ther Ori installation folder. Three other files are required to build the patch code, but not modfied.

**Q:** Script doesn't work, produces red text and warnings, etc.

**A:** Run script, right-click window title, select `Edit->Select All`, press `Enter`. This will copy all text on screen. Then post it to the [Steam Community] or [GitHub issues] and I'll try to help you.

###Final note
If the tool works for you, please, post your `Ori_Controller_Remap.ini` file and controller name to the [Steam Community] or [GitHub issues], so I can update my script with it.

[JoystickTest application]:http://www.planetpointy.co.uk/joystick-test-application
[Steam Community]:http://steamcommunity.com/app/261570/discussions
[GitHub issues]:https://github.com/beatcracker/Ori-Controller-Remap/issues