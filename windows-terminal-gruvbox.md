# Gruvbox theme for Windows Terminal

File `settings.json` example:
```json

// To view the default settings, hold "alt" while clicking on the "Settings" button.
// For documentation on these settings, see: https://aka.ms/terminal-documentation

{
    "$schema": "https://aka.ms/terminal-profiles-schema",

    "defaultProfile": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",

    "profiles":
    [
        {
            // Make changes here to the cmd.exe profile
            "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
            "name": "cmd",
            "commandline": "cmd.exe",
            "fontFace": "Noto Mono",
            "colorScheme": "Gruvbox Dark",
            "fontSize": 10,
            "hidden": false
        },
        {
            // Make changes here to the powershell.exe profile
            "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
            "name": "Windows PowerShell",
            "commandline": "powershell.exe",
            "fontFace": "Noto Mono",
            "fontSize": 10,
            "colorScheme": "flat-ui-v1",
            "hidden": false
        },
        {
            "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
            "hidden": false,
            "name": "Azure Cloud Shell",
            "source": "Windows.Terminal.Azure"
        },
        {
            "guid": "{c6eaf9f4-32a7-5fdc-b5cf-066e8a4b1e40}",
            "hidden": false,
            "name": "Ubuntu-18.04",
            "source": "Windows.Terminal.Wsl",
            "fontFace": "Noto Mono",
            "fontSize": 10,
            "colorScheme": "Gruvbox Dark"
        },
        {
            "guid": "{07b52e3e-de2c-5db4-bd2d-ba144ed6c273}",
            "hidden": false,
            "name": "Ubuntu-20.04",
            "source": "Windows.Terminal.Wsl"
        }
    ],

    // Add custom color schemes to this array
    "schemes": [
    {
    "background" : "#282828",
    "black" : "#282828",
    "blue" : "#458588",
    "brightBlack" : "#928374",
    "brightBlue" : "#83A598",
    "brightCyan" : "#8EC07C",
    "brightGreen" : "#B8BB26",
    "brightPurple" : "#D3869B",
    "brightRed" : "#FB4934",
    "brightWhite" : "#EBDBB2",
    "brightYellow" : "#FABD2F",
    "cyan" : "#689D6A",
    "foreground" : "#EBDBB2",
    "green" : "#98971A",
    "name" : "Gruvbox Dark",
    "purple" : "#B16286",
    "red" : "#CC241D",
    "white" : "#A89984",
    "yellow" : "#D79921"
  },
  {
   "background":"#000000",
   "black":"#000000",
   "blue":"#2980b9",
   "brightBlack":"#7f8c8d",
   "brightBlue":"#3498db",
   "brightCyan":"#1abc9c",
   "brightGreen":"#2ecc71",
   "brightPurple":"#9b59b6",
   "brightRed":"#e74c3c",
   "brightWhite":"#ecf0f1",
   "brightYellow":"#f1c40f",
   "cyan":"#16a085",
   "foreground":"#ecf0f1",
   "green":"#27ae60",
   "name":"flat-ui-v1",
   "purple":"#8e44ad",
   "red":"#c0392b",
   "white":"#ecf0f1",
   "yellow":"#f39c12"
  }
  ],

    // Add any keybinding overrides to this array.
    // To unbind a default keybinding, set the command to "unbound"
    "keybindings": []
}

```