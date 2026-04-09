How the UI Library Works
1. Create a Window
lua
local myWindow = Library:Window({
    Name = "My GUI",
    SubName = "Awesome features",
    Logo = "rbxassetid://1234567890"   -- optional image ID
})
2. Add Pages
lua
local page1 = myWindow:Page({
    Name = "Combat",
    Icon = "1234567890",    -- optional icon ID
    Columns = 2             -- number of columns inside the page (default 2)
})
3. Add Sections inside a Page
lua
local section = page1:Section({
    Name = "Aimbot",
    Description = "Settings for aim assistance",
    Icon = "0987654321",    -- optional
    Side = 1                -- which column (1 or 2, default 1)
})
4. Add UI Elements inside a Section
Toggle
lua
local toggle = section:Toggle({
    Name = "Enable Aimbot",
    Flag = "aimbot_enabled",      -- unique key for saving/loading
    Default = false,
    Callback = function(value)
        print("Aimbot is now", value)
    end
})
Slider
lua
section:Slider({
    Name = "Aim FOV",
    Flag = "aim_fov",
    Min = 0,
    Max = 360,
    Default = 90,
    Suffix = "°",
    Decimals = 1,
    Callback = function(value) end
})
Dropdown (single or multi)
lua
section:Dropdown({
    Name = "Weapon",
    Flag = "weapon",
    Items = {"AK47", "M4", "Sniper"},
    Default = "AK47",
    Multi = false,   -- set true for multiple selections
    Callback = function(selected) end
})
Keybind
lua
section:Keybind({
    Name = "Triggerbot",
    Flag = "trigger_key",
    Default = Enum.KeyCode.X,
    Mode = "Toggle",   -- "Toggle", "Hold", or "Always"
    Callback = function(pressed) end
})
Textbox
lua
section:Textbox({
    Flag = "message",
    Default = "",
    Placeholder = "Enter text...",
    Numeric = false,   -- true = only numbers allowed
    Finished = true,   -- true = callback fires when Enter pressed
    Callback = function(text) end
})
Button
lua
local btn = section:Button({
    Name = "Refresh",
    Icon = "1234567890",   -- optional
    Callback = function() print("Clicked") end
})
-- Manually trigger: btn:Press()
Label (simple text)
lua
local label = section:Label("Info text")
label:SetText("New text")
Listbox (searchable dropdown)
lua
local list = section:Listbox({
    Flag = "list",
    Items = {"Option1", "Option2"},
    Default = "Option1",
    Multi = false,
    Size = 200,   -- height of the listbox
    Callback = function(value) end
})
-- Add items dynamically: list:Add("Option3")
Colorpicker (attached to a Label or Toggle)
lua
local label = section:Label("Pick a color")
local picker = label:Colorpicker({
    Flag = "mycolor",
    Default = Color3.fromRGB(255,0,0),
    Alpha = true,   -- enable alpha slider
    Callback = function(color, alpha) end
})
5. Notifications
lua
Library:Notification({
    Title = "Success",
    Description = "Settings saved!",
    Duration = 3,
    Icon = "73789337996373",   -- optional image ID
    IconColor = {              -- optional gradient for icon
        Start = Color3.fromRGB(255,0,0),
        End = Color3.fromRGB(0,255,0)
    }
})
6. Global Chat (built‑in chat widget)
lua
local chat = myPage:GlobalChat(columnNumber)   -- 1 or 2

chat:OnMessageSendPressed(function()
    local msg = chat:GetTypedMessage()
    chat:SendMessage(avatarId, username, msg, isLocalPlayer)
    chat:ClearText()
end)

chat:SetStatus("Connected", Color3.fromRGB(0,255,0))
chat:SetVisibility(true)
7. Keybind List (floating window that shows active keybinds)
lua
local keyList = Library:KeybindList("Active Keybinds")

-- When you create a Keybind element, it automatically appears here.
-- You can manually add entries:
local entry = keyList:Add("ESP Toggle", "X")
entry:SetStatus(true)   -- highlight as active
entry:Set("New Name", "Z")
8. Settings Page (built‑in config management)
lua
local settingsPage = Library:CreateSettingsPage(myWindow, keyList)
-- This adds a page with config saving/loading (JSON files in "lyapossss/Configs")
9. Saving & Loading Configs (automatically handled by settings page)
All elements with a Flag are saved/loaded. Use the settings page UI or call manually:

lua
local configJSON = Library:GetConfig()
writefile("myconfig.json", configJSON)
Library:LoadConfig(readfile("myconfig.json"))
10. Unloading the UI
lua
Library:Unload()   -- destroys GUI and disconnects all events
Important Notes
The library uses gethui() or CoreGui as parent. It creates folders lyapossss/Configs etc. for storing configs.

All UI elements support theming. You can change the accent color at runtime:

lua
Library:ChangeTheme("Accent", Color3.fromRGB(255,0,0))
The library automatically adapts to mobile (touch events, scaling).

Fonts are loaded from Roblox assets; you can change the font weight in the settings page.

After removing the demo code, you can copy the entire library script into your executor and use the above API to build your own GUI.
