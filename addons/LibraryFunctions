local libraryFunctions = {}

-- services
local stats = game:GetService("Stats")
local runService = game:GetService("RunService")

-- variables
local network = stats.Network
local serverStatsItem = stats.serverStatsItem

function libraryFunctions.init(library, saveManager, themeManager, tabs, libName)
	saveManager:SetLibrary(library)
	saveManager:IgnoreThemeSettings()
	saveManager:SetFolder(libName)

	themeManager:SetLibrary(library)
	themeManager:ApplyToTab(tabs.settings)
end

function libraryFunctions.watermark(library, saveManager, libName)
	library:SetWatermarkVisibility(true)

	local frameTimer = tick()
	local frameCounter = 0
	local framesPerSecond = 60

	local watermarkConnection = runService.RenderStepped:Connect(function()
		frameCounter += 1

		if (tick() - frameTimer) >= 1 then
			framesPerSecond = frameCounter
			frameTimer = tick()
			frameCounter = 0
		end

		library:SetWatermark(
			(libName..' | %s fps | %s networkPing (ms)')
			
			:format(
				math.floor(framesPerSecond),
				math.floor(serverStatsItem['Data Ping']:GetValue())
			)
		)
	end)

	library:OnUnload(function()
		watermarkConnection:Disconnect()
		library.Unloaded = true
	end)

	saveManager:LoadAutoloadConfig()
end

function libraryFunctions.toggling(library, saveManager, tabs)
	local keybindsBox = tabs.settings:AddRightGroupbox("Keybinds")
	saveManager:BuildConfigSection(tabs.settings)
	keybindsBox:AddLabel('Menu Toggle Keybind'):AddKeyPicker('MenuKeybind', { Default = 'End', NoUI = true, Text = 'Menu keybind' })

	local unloadButton = keybindsBox:AddButton({
		Text = 'Unload UI',
		Func = function()
			library:Unload()
		end,
		DoubleClick = true,
		Tooltip = 'Unloads the UI, click twice to confirm.'
	})

	library.ToggleKeybind = Options.MenuKeybind
end

return libraryFunctions
