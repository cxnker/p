local ClientSource = {
	{
		PlacesIds = {4924922222}, --gui
		ScriptUrl = "https://raw.githubusercontent.com/realgengar/scripts/refs/heads/main/Gui%20Version.Lua", true,
		Enabled = true
	},
	{
		PlacesIds = {3101667897},--veloz
		ScriptUrl = "https://raw.githubusercontent.com/realgengar/SpeedLegends-/refs/heads/main/Source.lua", true,
		Enabled = true
	},
{
		PlacesIds = {10260193230}, --meme
		ScriptUrl = "https://raw.githubusercontent.com/realgengar/MemeSea/refs/heads/main/Source.lua",
		Enabled = true
	},
{
		PlacesIds = {13864661000}, --breck
		ScriptUrl = "https://raw.githubusercontent.com/realgengar/BreakIn2/refs/heads/main/Source.lua",
		Enabled = true
	}
}

local DripUniversal = "https://raw.githubusercontent.com/realgengar/scripts/refs/heads/main/Games.Lua" -- Universo

local fetcher = {}
local _ENV = (getgenv or getrenv or getfenv)()
do
	local last_exec = _ENV.script_execute_debounce
	
	if last_exec and (os.clock() - last_exec) <= 3 then
		print("up constantemente")
		return nil
	end
	
	_ENV.script_execute_debounce = os.clock()
end

local function CreateErrorMessage(Text)
	_ENV.loadedScript = nil
	_ENV.OnScript = false
	local Message = Instance.new("Message", workspace)
	Message.Text = Text
	_ENV.error_message = Message
	game:GetService("Debris"):AddItem(Message, 5)
	error(Text, 2)
end

function fetcher.get(Url)
	local success, response = pcall(function()
		return game:HttpGet(Url)
	end)
	
	if success then
		return response
	else
		CreateErrorMessage("parou miséria: " .. Url .. "\nErro: " .. tostring(response))
	end
end

-- Função para carregar e executar script
function fetcher.load(Url)
	print("carrega logo: " .. Url)
	local raw = fetcher.get(Url)
	if not raw then return end
	
	local runFunction, errorText = loadstring(raw)
	
	if type(runFunction) ~= "function" then
		CreateErrorMessage("outro erro aaaa: " .. Url .. "\nErro: " .. tostring(errorText))
	else
		return runFunction
	end
end

local function IsValidPlace(Script)
	if not Script.Enabled then
		return false
	end
	
	if Script.PlacesIds then
		for _, placeId in ipairs(Script.PlacesIds) do
			if placeId == game.PlaceId then
				return true
			end
		end
	end
	
	return false
end

local function HasSpecificScript()
	for _, Script in ipairs(ClientSource) do
		if IsValidPlace(Script) then
			return true, Script
		end
	end
	return false, nil
end

local function GetGameInfo()
	local gameInfo = {
		PlaceId = game.PlaceId,
		GameId = game.GameId,
		JobId = game.JobId,
		Name = game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name or "Desconhecido"
	}
	
	return gameInfo
end

local function ExecuteUniversalScript()
	local gameInfo = GetGameInfo()
	print("script universal: " .. gameInfo.Name .. " (ID: " .. gameInfo.PlaceId .. ")")
	print("carregandu de: " .. DripUniversal)
	
	local scriptFunction = fetcher.load(DripUniversal)
	if scriptFunction then
		local success, result = pcall(scriptFunction)
		if success then
			print("success aiaiai")
			_ENV.loadedScript = true
			_ENV.OnScript = true
			
			local Message = Instance.new("Message", workspace)
			Message.Text = "Script Universal"
			game:GetService("Debris"):AddItem(Message, 3)
			
			return true
		else
			CreateErrorMessage("erro egua: " .. tostring(result))
		end
	end
	return false
end

local function ExecuteSpecificScript(Script)
	print("caregando nossa senhora")
	print("aqui carregadu: " .. Script.ScriptUrl)
	
	local scriptFunction = fetcher.load(Script.ScriptUrl)
	if scriptFunction then
		local success, result = pcall(scriptFunction)
		if success then
			print("deu bom")
			_ENV.loadedScript = true
			_ENV.OnScript = true
			return true
		else
			CreateErrorMessage("deu erro dnv: " .. tostring(result))
		end
	end
	return false
end

local function CarrgarDripp()
	local gameInfo = GetGameInfo()
	print("Achouuu: " .. gameInfo.Name .. " (ID: " .. gameInfo.PlaceId .. ")")
	
	local hasSpecific, specificScript = HasSpecificScript()
	
	if hasSpecific then
		print("tem exp")
		return ExecuteSpecificScript(specificScript)
	else
		print("nn certo")
		return ExecuteUniversalScript()
	end
end

local function ShowSupportedGames()
	local supportedGames = {}
	for _, Script in ipairs(ClientSource) do
		if Script.Enabled then
			for _, placeId in ipairs(Script.PlacesIds) do
				local success, info = pcall(function()
					return game:GetService("MarketplaceService"):GetProductInfo(placeId).Name
				end)
				if success then
					table.insert(supportedGames, info .. " (" .. placeId .. ")")
				end
			end
		end
	end
	
	print(table.concat(supportedGames, "\n"))
end

-- Sistema de auto-execução em teleporte (opcional)
--[[
do
	local executor = syn or fluxus
	local queueteleport = queue_on_teleport or (executor and executor.queue_on_teleport)
	
	if not _ENV.added_teleport_queue and type(queueteleport) == "function" then
		_ENV.added_teleport_queue = true
		
		local scriptCode = string.format("loadstring(game:HttpGet('%s'))()", "https://raw.githubusercontent.com/SeuUsuario/SeuRepositorio/main/script-loader.lua")
		pcall(queueteleport, scriptCode)
		print("miséria caregadaaaa!")
	end
end
--]]

return CarrgarDripp()
