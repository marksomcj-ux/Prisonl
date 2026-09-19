--[[ Prison Life • v13.0 • MUSIC + CUSTOM ID (Delta Mobile) ]]

local P,R,U,L = game:GetService("Players"), game:GetService("RunService"),
                game:GetService("UserInputService"), game:GetService("Lighting")
local LP, C = P.LocalPlayer, workspace.CurrentCamera

local cfg = {
    esp=1, rc=1, hst=1, nm=1, tr=1, trb=1, thick=1.5,
    aim=0, fov=250, sm=.25, tgt="Head", circ=1, fb=0,
    infJump=0, sprint=16
}
local RC = {
    Cr=Color3.fromRGB(255,50,50), Gu=Color3.fromRGB(50,120,255),
    In=Color3.fromRGB(255,140,0), Ho=Color3.fromRGB(180,100,255),
    Df=Color3.fromRGB(255,255,255)
}
local UI = {
    bg=Color3.fromRGB(15,15,15), off=Color3.fromRGB(45,45,45),
    on=Color3.fromRGB(200,200,200), td=Color3.fromRGB(15,15,15),
    tl=Color3.fromRGB(230,230,230), lb=Color3.fromRGB(140,140,140),
    bd=Color3.fromRGB(90,90,90), st=Color3.fromRGB(255,255,255)
}

-- ═══ ХЕЛПЕРЫ ═══
local function getChar() return LP.Character end
local function getHRP() local c=getChar() return c and c:FindFirstChild("HumanoidRootPart") end
local function getHum() local c=getChar() return c and c:FindFirstChildOfClass("Humanoid") end
local function getMouse() return LP:GetMouse() end

local function cleanOld(name)
    local bp = LP:FindFirstChildOfClass("Backpack")
    local ch = getChar()
    if bp then local o=bp:FindFirstChild(name) if o then o:Destroy() end end
    if ch then local o=ch:FindFirstChild(name) if o then o:Destroy() end end
end
local function makeTool(name, tip)
    cleanOld(name)
    local bp = LP:FindFirstChildOfClass("Backpack")
    if not bp then return nil end
    local t = Instance.new("Tool")
    t.Name = name; t.RequiresHandle = false
    t.CanBeDropped = false; t.ToolTip = tip or name
    t.Parent = bp
    return t
end
local function pointFromClick(input)
    local m = getMouse()
    if m and m.Hit then
        local d = (m.Hit.Position - C.CFrame.Position).Magnitude
        if d > 1 then return m.Hit.Position end
    end
    if input then
        local ray = C:ViewportPointToRay(input.Position.X, input.Position.Y)
        local res = workspace:Raycast(ray.Origin, ray.Direction * 5000)
        if res then return res.Position end
        return ray.Origin + ray.Direction * 100
    end
    return nil
end
local function getAimTarget()
    local m = getMouse()
    if m and m.Target then
        local p = m.Target.Parent
        if p then
            local h = p:FindFirstChildOfClass("Humanoid")
            if h then return h, p end
        end
    end
    return nil
end

-- ═══ РОЛИ ═══
local function team(p,pat) local t=p.Team if not t then return false end
    return t.Name:lower():find(pat)~=nil end
local function role(p)
    if team(p,"guard") or team(p,"police") then return "Gu" end
    if team(p,"criminal") or team(p,"escaped") then return "Cr" end
    if team(p,"inmate") or team(p,"prisoner") then return "In" end
end
local function hostile(p)
    if not cfg.hst or not p.Character then return false end
    if p.Character:GetAttribute("Hostile") or p:GetAttribute("Hostile") then return true end
    local s=p.Character:GetAttribute("Status") or p:GetAttribute("Status")
    if s=="Hostile" then return true end
    for _,d in ipairs(p.Character:GetDescendants()) do
        if d:IsA("TextLabel") and (d.Text:find("❗") or d.Text:lower():find("hostile")) then return true end
    end
    return false
end
local function col(p)
    if cfg.rc==0 then return RC.Df end
    local r=role(p) if not r then return RC.Df end
    if r=="In" and hostile(p) then return RC.Ho end
    return RC[r] or RC.Df
end
local function en(p)
    local r=role(LP) if not r then return false end
    if r=="Gu" then return team(p,"criminal") or team(p,"escaped") or hostile(p) end
    return team(p,"guard") or team(p,"police")
end

-- ═══ TROLL ═══
local function tpToRandomRole(roleKey)
    local c = {}
    for _,p in ipairs(P:GetPlayers()) do
        if p~=LP and p.Character then
            if role(p)==roleKey then
                local hrp=p.Character:FindFirstChild("HumanoidRootPart")
                local hum=p.Character:FindFirstChildOfClass("Humanoid")
                if hrp and hum and hum.Health>0 then table.insert(c,hrp) end
            end
        end
    end
    if #c==0 then return false end
    local myHrp = getHRP()
    if not myHrp then return false end
    local t = c[math.random(1,#c)]
    myHrp.CFrame = CFrame.new(t.Position + Vector3.new(0,3,0))
    return true
end
local function tpCoords(x,yy,z)
    local hrp = getHRP()
    if not hrp then return false end
    hrp.CFrame = CFrame.new(x, yy+5, z)
    return true
end

-- ═══ МУЗЫКА ═══
local musicSnd = Instance.new("Sound")
musicSnd.Name = "BG_Music"
musicSnd.Volume = 0.5
musicSnd.Looped = true
musicSnd.Parent = game:GetService("SoundService")

local musicTracks = {
    {name = "The Great Strategy",   id = "rbxassetid://6136889498"},
    {name = "Clair De Lune",        id = "rbxassetid://1838457617"},
    {name = "Nutcracker Suite",     id = "rbxassetid://1846627783"},
    {name = "Wooden Bear",          id = "rbxassetid://1844397736"},
    {name = "Get Hyper",            id = "rbxassetid://138855854"},
    {name = "Raining Tacos",        id = "rbxassetid://142376088"},
    {name = "Rasputin",             id = "rbxassetid://5512350519"},
    {name = "Take On Me",           id = "rbxassetid://4606705490"},
    {name = "Gangsta's Paradise",   id = "rbxassetid://6070263388"},
    {name = "Smooth Criminal",      id = "rbxassetid://4883181281"},
    {name = "Believer",             id = "rbxassetid://2389193148"},
    {name = "Heat Waves",           id = "rbxassetid://6432181830"},
    {name = "Levitating",           id = "rbxassetid://6606223785"},
    {name = "Crab Rave",            id = "rbxassetid://5410086218"},
    {name = "Paradise Falls",       id = "rbxassetid://1837879082"},
}
local musicIdx = 1
local musicPlaying = false
local customId = nil

local function musicStart()
    musicPlaying = true
    if customId then
        musicSnd.SoundId = customId
    else
        musicSnd.SoundId = musicTracks[musicIdx].id
    end
    musicSnd:Play()
end
local function musicStop()
    musicPlaying = false
    musicSnd:Stop()
end
local function musicSelect(i)
    musicIdx = i
    customId = nil
    if musicPlaying then
        musicSnd:Stop()
        musicSnd.SoundId = musicTracks[i].id
        musicSnd:Play()
    end
end

-- ═══ TOOLS ═══
local function giveTP()
    local t=makeTool("TP_Tool","Tap to TP") if not t then return end
    t.Activated:Connect(function()
        local pt=pointFromClick(nil)
        if pt then local hrp=getHRP()
            if hrp then hrp.CFrame=CFrame.new(pt+Vector3.new(0,3,0)) end end
    end)
end
local function giveSpeed()
    local t=makeTool("Speed_Tool","Speed 100") if not t then return end
    t.Equipped:Connect(function() local h=getHum() if h then h.WalkSpeed=100 end end)
    t.Unequipped:Connect(function() local h=getHum() if h then h.WalkSpeed=cfg.sprint end end)
end
local function giveFly()
    local t=makeTool("Fly_Tool","Joystick + jump") if not t then return end
    local conn=nil local up=false
    U.JumpRequest:Connect(function() if conn then up=true end end)
    U.InputEnded:Connect(function(i) if i.KeyCode==Enum.KeyCode.Space then up=false end end)
    t.Equipped:Connect(function()
        local h=getHum() if h then h.WalkSpeed=0 end
        conn=R.RenderStepped:Connect(function(dt)
            local hrp=getHRP() local hu=getHum()
            if not hrp or not hu then return end
            local spd=60*dt
            if hu.MoveDirection.Magnitude>0 then
                hrp.CFrame=hrp.CFrame+hu.MoveDirection.Unit*spd end
            if up then hrp.CFrame=hrp.CFrame+Vector3.new(0,spd,0) end
        end)
    end)
    t.Unequipped:Connect(function()
        if conn then conn:Disconnect() conn=nil end
        up=false
        local h=getHum() if h then h.WalkSpeed=cfg.sprint end
    end)
end
local function giveSpin()
    local t=makeTool("Spin_Tool","Spin") if not t then return end
    local conn=nil
    t.Equipped:Connect(function()
        conn=R.Heartbeat:Connect(function()
            local hrp=getHRP()
            if hrp then hrp.CFrame=hrp.CFrame*CFrame.Angles(0,math.rad(30),0) end
        end)
    end)
    t.Unequipped:Connect(function() if conn then conn:Disconnect() conn=nil end end)
end
local function giveSize()
    local t=makeTool("Size_Tool","Size x2") if not t then return end
    local orig={}
    t.Equipped:Connect(function()
        local c=getChar() if not c then return end
        orig={}
        for _,p in ipairs(c:GetDescendants()) do
            if p:IsA("BasePart") then orig[p]=p.Size p.Size=p.Size*2 end end
    end)
    t.Unequipped:Connect(function()
        for p,s in pairs(orig) do if p and p.Parent then p.Size=s end end
        orig={}
    end)
end
local function giveBoombox()
    local t=makeTool("Boombox_Tool","Music") if not t then return end
    local snd=nil
    t.Equipped:Connect(function()
        local hrp=getHRP() if not hrp then return end
        snd=Instance.new("Sound",hrp)
        snd.SoundId="rbxassetid://1837879082"
        snd.Volume=2 snd.Looped=true snd:Play()
    end)
    t.Unequipped:Connect(function() if snd then snd:Destroy() snd=nil end end)
end
local function giveSpectate()
    local t=makeTool("Spectate_Tool","Tap player") if not t then return end
    t.Activated:Connect(function()
        local hum=getAimTarget()
        if hum then C.CameraSubject=hum end
    end)
    t.Unequipped:Connect(function()
        local h=getHum() if h then C.CameraSubject=h end
    end)
end
local function giveRejoin()
    local t=makeTool("Rejoin_Tool","Rejoin") if not t then return end
    t.Activated:Connect(function()
        local TS=game:GetService("TeleportService")
        pcall(function() TS:TeleportToPlaceInstance(game.PlaceId,game.JobId) end)
    end)
end
local function giveServerHop()
    local t=makeTool("ServerHop_Tool","Hop") if not t then return end
    t.Activated:Connect(function()
        local TS=game:GetService("TeleportService")
        pcall(function() TS:Teleport(game.PlaceId) end)
    end)
end

-- ═══ ESP ═══
local ES={}
local function aESP(p,ch)
    if not ch or p==LP then return end
    local c=col(p)
    if ES[p] and ES[p].h and ES[p].h.Parent then
        ES[p].h.FillColor=c ES[p].h.OutlineColor=c
        if ES[p].l then ES[p].l.TextColor3=c end return
    end
    local h=Instance.new("Highlight",ch) h.Adornee=ch
    h.DepthMode=Enum.HighlightDepthMode.AlwaysOnTop
    h.FillColor=c h.FillTransparency=1 h.OutlineColor=c h.OutlineTransparency=0
    local bb=Instance.new("BillboardGui",ch)
    bb.Adornee=ch:FindFirstChild("Head") or ch:FindFirstChild("HumanoidRootPart")
    bb.Size=UDim2.new(0,120,0,20) bb.StudsOffset=Vector3.new(0,3,0)
    bb.AlwaysOnTop=true bb.Enabled=cfg.nm==1
    local l=Instance.new("TextLabel",bb)
    l.Size=UDim2.new(1,0,1,0) l.BackgroundTransparency=1 l.TextColor3=c
    l.TextStrokeTransparency=0 l.TextStrokeColor3=Color3.new()
    l.TextScaled=true l.Font=Enum.Font.GothamBold l.Text=p.Name
    ES[p]={h=h,bb=bb,l=l}
end
local function dESP(p)
    if ES[p] then
        if ES[p].h then ES[p].h:Destroy() end
        if ES[p].bb then ES[p].bb:Destroy() end
        ES[p]=nil
    end
end
local function refESP()
    for _,p in ipairs(P:GetPlayers()) do
        if p~=LP then dESP(p) if p.Character then aESP(p,p.Character) end end
    end
end

-- ═══ ТРАССЕРЫ ═══
local TR={}
local function gTr(p)
    if TR[p] then return TR[p] end
    local l=Drawing.new("Line")
    l.Thickness=cfg.thick l.Transparency=1 l.Visible=false
    TR[p]=l return l
end
local FOV=Drawing.new("Circle")
FOV.Thickness=1.5 FOV.Color=Color3.fromRGB(220,220,220)
FOV.Filled=false FOV.Transparency=1 FOV.NumSides=60 FOV.Visible=false

-- ═══ АВТОПРИЦЕЛ ═══
local function aimT()
    if cfg.aim==0 then return nil end
    local cx,cy=C.ViewportSize.X/2,C.ViewportSize.Y/2
    local best,bd=nil,cfg.fov
    for _,p in ipairs(P:GetPlayers()) do
        if p~=LP and en(p) then
            local ch=p.Character
            local hm=ch and ch:FindFirstChildOfClass("Humanoid")
            if hm and hm.Health>0 then
                local pt=ch:FindFirstChild(cfg.tgt)
                if pt then
                    local sp,on=C:WorldToViewportPoint(pt.Position)
                    if on and sp.Z>0 then
                        local d=math.sqrt((sp.X-cx)^2+(sp.Y-cy)^2)
                        if d<bd then bd=d best=pt end
                    end
                end
            end
        end
    end
    return best
end

local O={L.Ambient,L.OutdoorAmbient,L.Brightness,L.FogEnd,L.ClockTime,L.GlobalShadows}
local function fb(s)
    if s then
        L.Ambient=Color3.fromRGB(220,220,220) L.OutdoorAmbient=Color3.fromRGB(220,220,220)
        L.Brightness=5 L.FogEnd=1e6 L.ClockTime=14 L.GlobalShadows=false
    else
        L.Ambient=O[1] L.OutdoorAmbient=O[2] L.Brightness=O[3]
        L.FogEnd=O[4] L.ClockTime=O[5] L.GlobalShadows=O[6]
    end
end

local infJumpConn=nil
local function setInfJump(state)
    if infJumpConn then infJumpConn:Disconnect() infJumpConn=nil end
    if state then
        infJumpConn=U.JumpRequest:Connect(function()
            local c=LP.Character if not c then return end
            local h=c:FindFirstChildOfClass("Humanoid") if not h then return end
            h:ChangeState(Enum.HumanoidStateType.Jumping)
            task.wait(0.12)
            if h and h.Parent then h:ChangeState(Enum.HumanoidStateType.Freefall) end
        end)
    end
end

local function applySpeed()
    local ch=LP.Character
    if ch then
        local hum=ch:FindFirstChildOfClass("Humanoid")
        if hum then hum.WalkSpeed=cfg.sprint end
    end
end

-- ═══ ПЛАВАЮЩАЯ КНОПКА AIM ═══
local QG=Instance.new("ScreenGui",game.CoreGui)
QG.ResetOnSpawn=false QG.DisplayOrder=999
local QB=Instance.new("TextButton",QG)
QB.Size=UDim2.new(0,70,0,70) QB.Position=UDim2.new(0,20,.5,-35)
QB.BackgroundColor3=UI.off QB.Text="AIM\nOFF" QB.TextColor3=UI.tl
QB.Font=Enum.Font.GothamBold QB.TextScaled=true QB.AutoButtonColor=false
Instance.new("UICorner",QB).CornerRadius=UDim.new(1,0)
local qs=Instance.new("UIStroke",QB)
qs.Color=UI.st qs.Thickness=2 qs.Transparency=.4

local function makeDraggable(b)
    local dr,ds,sp0=false,nil,nil
    b.InputBegan:Connect(function(i)
        if i.UserInputType==Enum.UserInputType.Touch or i.UserInputType==Enum.UserInputType.MouseButton1 then
            dr=true ds=i.Position sp0=b.Position
            i.Changed:Connect(function() if i.UserInputState==Enum.UserInputState.End then dr=false end end)
        end
    end)
    b.InputChanged:Connect(function(i)
        if dr and (i.UserInputType==Enum.UserInputType.Touch or i.UserInputType==Enum.UserInputType.MouseMovement) then
            local d=i.Position-ds
            b.Position=UDim2.new(sp0.X.Scale,sp0.X.Offset+d.X,sp0.Y.Scale,sp0.Y.Offset+d.Y)
        end
    end)
end
makeDraggable(QB)

-- ═══ МЕНЮ ═══
local SG=Instance.new("ScreenGui",game.CoreGui) SG.ResetOnSpawn=false
local M=Instance.new("Frame",SG)
M.Size=UDim2.new(0,330,0,480) M.Position=UDim2.new(.5,-165,.5,-240)
M.BackgroundColor3=UI.bg M.Active=true M.Draggable=true
Instance.new("UICorner",M).CornerRadius=UDim.new(0,12)
local ms=Instance.new("UIStroke",M)
ms.Color=UI.st ms.Thickness=1.5 ms.Transparency=.25
local T=Instance.new("TextLabel",M)
T.Size=UDim2.new(1,-40,0,40) T.BackgroundTransparency=1
T.Text="PRISON LIFE • v13.0" T.TextColor3=UI.tl T.Font=Enum.Font.GothamBold T.TextSize=14
local CB=Instance.new("TextButton",M)
CB.Size=UDim2.new(0,30,0,30) CB.Position=UDim2.new(1,-35,0,5)
CB.BackgroundColor3=UI.off CB.Text="-" CB.TextColor3=UI.tl
CB.Font=Enum.Font.GothamBold CB.TextSize=18 CB.AutoButtonColor=false
Instance.new("UICorner",CB).CornerRadius=UDim.new(0,6)

local Scroll=Instance.new("ScrollingFrame",M)
Scroll.Size=UDim2.new(1,-20,1,-50) Scroll.Position=UDim2.new(0,10,0,45)
Scroll.BackgroundTransparency=1 Scroll.BorderSizePixel=0
Scroll.ScrollBarThickness=6 Scroll.ScrollBarImageColor3=UI.bd
Scroll.CanvasSize=UDim2.new(0,0,0,0)
Scroll.AutomaticCanvasSize=Enum.AutomaticSize.Y
Scroll.ScrollingDirection=Enum.ScrollingDirection.Y

local function btn(t,y)
    local b=Instance.new("TextButton",Scroll)
    b.Size=UDim2.new(1,-10,0,34) b.Position=UDim2.new(0,0,0,y)
    b.BackgroundColor3=UI.off b.Text=t b.TextColor3=UI.tl
    b.Font=Enum.Font.GothamBold b.TextSize=12 b.AutoButtonColor=false
    Instance.new("UICorner",b).CornerRadius=UDim.new(0,8)
    local s=Instance.new("UIStroke",b)
    s.Color=UI.bd s.Thickness=1 s.Transparency=.4
    return b
end
local function lbl(t,y)
    local l=Instance.new("TextLabel",Scroll)
    l.Size=UDim2.new(1,-10,0,20) l.Position=UDim2.new(0,0,0,y)
    l.BackgroundTransparency=1 l.Text=t l.TextColor3=UI.lb
    l.Font=Enum.Font.GothamBold l.TextSize=11
    l.TextXAlignment=Enum.TextXAlignment.Left
    return l
end
local function st(b,on)
    b.BackgroundColor3=on and UI.on or UI.off
    b.TextColor3=on and UI.td or UI.tl
end
local function tog(key,b,t1,t2,extra)
    b.MouseButton1Click:Connect(function()
        cfg[key]=1-cfg[key]
        b.Text=cfg[key]==1 and t1 or t2
        st(b,cfg[key]==1)
        if extra then extra() end
    end)
end

local y=4
local function step(h) y=y+(h or 38) end

-- ESP
lbl("── ESP ──",y) step(22)
local b1=btn("ESP: ВКЛ",y) st(b1,true) step()
tog("esp",b1,"ESP: ВКЛ","ESP: ВЫКЛ",function()
    if cfg.esp==1 then refESP() else for p in pairs(ES) do dESP(p) end end
end)
local b2=btn("Ролевые цвета: ВКЛ",y) st(b2,true) step()
tog("rc",b2,"Ролевые цвета: ВКЛ","Ролевые цвета: ВЫКЛ",refESP)
local b3=btn("Враждебные: ВКЛ",y) st(b3,true) step()
tog("hst",b3,"Враждебные: ВКЛ","Враждебные: ВЫКЛ",refESP)
local b4=btn("Имена: ВКЛ",y) st(b4,true) step()
tog("nm",b4,"Имена: ВКЛ","Имена: ВЫКЛ",function()
    for _,d in pairs(ES) do if d.bb then d.bb.Enabled=cfg.nm==1 end end
end)

-- Полосы
lbl("── Полосы ──",y) step(22)
local b5=btn("Полосы: ВКЛ",y) st(b5,true) step()
tog("tr",b5,"Полосы: ВКЛ","Полосы: ВЫКЛ")
local b6=btn("Начало: НИЗ",y) st(b6,true) step()
tog("trb",b6,"Начало: НИЗ","Начало: ЦЕНТР")
local TH={1,1.5,2,3,4} local ti=2
local b7=btn("Толщина: 1.5",y) st(b7,true) step()
b7.MouseButton1Click:Connect(function()
    ti=ti%#TH+1 cfg.thick=TH[ti]
    b7.Text="Толщина: "..cfg.thick
    for _,l in pairs(TR) do l.Thickness=cfg.thick end
end)

-- Автоприцел
lbl("── Автоприцел ──",y) step(22)
local b8=btn("Автоприцел: ВЫКЛ",y) step()
tog("aim",b8,"Автоприцел: ВКЛ","Автоприцел: ВЫКЛ",function()
    QB.Text=cfg.aim==1 and "AIM\nON" or "AIM\nOFF"
    QB.BackgroundColor3=cfg.aim==1 and UI.on or UI.off
    QB.TextColor3=cfg.aim==1 and UI.td or UI.tl
end)
local b9=btn("Fullbright: ВЫКЛ",y) step()
tog("fb",b9,"Fullbright: ВКЛ","Fullbright: ВЫКЛ",function() fb(cfg.fb==1) end)

-- Движение
lbl("── Движение ──",y) step(22)
local b10=btn("Бесконечный прыжок: ВЫКЛ",y) step()
tog("infJump",b10,"Бесконечный прыжок: ВКЛ","Бесконечный прыжок: ВЫКЛ",
    function() setInfJump(cfg.infJump==1) end)
local b11=btn("Скорость бега: 16",y) step()
b11.MouseButton1Click:Connect(function()
    cfg.sprint=cfg.sprint+10
    if cfg.sprint>200 then cfg.sprint=16 end
    b11.Text="Скорость бега: "..cfg.sprint
    applySpeed()
end)

-- ТРОЛЛ
lbl("── ТРОЛЛ ──",y) step(22)
local function addTrollBtn(name, func)
    local b = btn(name, y); step()
    b.MouseButton1Click:Connect(function()
        local ok, res = pcall(func)
        if ok and res then b.Text = "+ " .. name else b.Text = "- Не найдено" end
        task.wait(1.2); b.Text = name
    end)
end
addTrollBtn("ТП к рандомному полицейскому", function() return tpToRandomRole("Gu") end)
addTrollBtn("ТП к рандомному заключённому", function() return tpToRandomRole("In") end)
addTrollBtn("ТП к рандомному сбежавшему", function() return tpToRandomRole("Cr") end)

-- Телепорты
lbl("── Телепорты ──",y) step(22)
local function addTPBtn(name, x, yy, z)
    local b = btn(name, y); step()
    b.MouseButton1Click:Connect(function()
        local ok = tpCoords(x, yy, z)
        if ok then b.Text = "+ " .. name else b.Text = "- Ошибка" end
        task.wait(1.2); b.Text = name
    end)
end
addTPBtn("Тюремные камеры",     917, 97, 2435)
addTPBtn("Тюремная крыша",      959, 125, 2469)
addTPBtn("Зона полицейских",    801, 97, 2299)
addTPBtn("Труба",               917, 87, 2105)
addTPBtn("База сбежавших",     -886, 101, 1949)
addTPBtn("Безопасная зона",    -884, 100, 1911)

-- МУЗЫКА
lbl("── Музыка ──", y) step(22)

local musicBtn = btn("Музыка: ВЫКЛ", y); step()
musicBtn.MouseButton1Click:Connect(function()
    if musicPlaying then
        musicStop()
        musicBtn.Text = "Музыка: ВЫКЛ"
        st(musicBtn, false)
    else
        musicStart()
        musicBtn.Text = "Музыка: ВКЛ"
        st(musicBtn, true)
    end
end)

local curTrackBtn = btn("Трек: " .. musicTracks[1].name, y); step()
curTrackBtn.MouseButton1Click:Connect(function()
    musicIdx = musicIdx % #musicTracks + 1
    musicSelect(musicIdx)
    curTrackBtn.Text = "Трек: " .. musicTracks[musicIdx].name
end)

local musicVolBtn = btn("Громкость: 50%", y); step()
local mVols = {0.1, 0.3, 0.5, 0.7, 1.0}
local mVolIdx = 3
musicVolBtn.MouseButton1Click:Connect(function()
    mVolIdx = mVolIdx % #mVols + 1
    musicSnd.Volume = mVols[mVolIdx]
    musicVolBtn.Text = "Громкость: " .. math.floor(mVols[mVolIdx] * 100) .. "%"
end)

-- Ввод своего ID
local idBox = Instance.new("TextBox", Scroll)
idBox.Size = UDim2.new(1,-10,0,34) idBox.Position = UDim2.new(0,0,0,y)
idBox.BackgroundColor3 = UI.off idBox.Text = ""
idBox.PlaceholderText = "Введи свой ID (только цифры)"
idBox.PlaceholderColor3 = UI.lb
idBox.TextColor3 = UI.tl idBox.Font = Enum.Font.GothamBold idBox.TextSize = 11
idBox.ClearTextOnFocus = false
Instance.new("UICorner", idBox).CornerRadius = UDim.new(0,8)
local idStroke = Instance.new("UIStroke", idBox)
idStroke.Color = UI.bd idStroke.Thickness = 1 idStroke.Transparency = .4
step()

local applyIdBtn = btn("Применить свой ID", y); step()
applyIdBtn.MouseButton1Click:Connect(function()
    local num = idBox.Text:gsub("%D", "")
    if num == "" then
        applyIdBtn.Text = "- Пусто"
        task.wait(1); applyIdBtn.Text = "Применить свой ID"
        return
    end
    customId = "rbxassetid://" .. num
    if musicPlaying then
        musicSnd:Stop()
        musicSnd.SoundId = customId
        musicSnd:Play()
    else
        musicStart()
        musicBtn.Text = "Музыка: ВКЛ"
        st(musicBtn, true)
    end
    curTrackBtn.Text = "Трек: Свой ID"
    applyIdBtn.Text = "+ Применено"
    task.wait(1); applyIdBtn.Text = "Применить свой ID"
end)

-- Инструменты
lbl("── Инструменты ──",y) step(22)
local function addToolBtn(name, func)
    local b = btn(name, y); step()
    b.MouseButton1Click:Connect(function()
        local ok = pcall(func)
        if ok then b.Text = "+ " .. name else b.Text = "- Ошибка" end
        task.wait(1); b.Text = name
    end)
end
addToolBtn("Выдать TP Tool",       giveTP)
addToolBtn("Выдать Speed Tool",    giveSpeed)
addToolBtn("Выдать Fly Tool",      giveFly)
addToolBtn("Выдать Spin Tool",     giveSpin)
addToolBtn("Выдать Size Tool",     giveSize)
addToolBtn("Выдать Boombox Tool",  giveBoombox)
addToolBtn("Выдать Spectate Tool", giveSpectate)
addToolBtn("Выдать Rejoin Tool",   giveRejoin)
addToolBtn("Выдать ServerHop Tool",giveServerHop)

-- Настройки аима
lbl("── Настройки аима ──",y) step(22)
local FV={80,150,250,400,600} local fi=3
local fa=btn("FOV: 250",y) st(fa,true) step()
fa.MouseButton1Click:Connect(function()
    fi=fi%#FV+1 cfg.fov=FV[fi] fa.Text="FOV: "..cfg.fov
end)
local SM={
    {n="ОЧЕНЬ МЯГКО",v=.05},{n="МЯГКО",v=.12},{n="СРЕДНЯЯ",v=.25},
    {n="БЫСТРО",v=.5},{n="МГНОВЕННО",v=1}
} local si=3
local fs=btn("Плавность: СРЕДНЯЯ",y) st(fs,true) step()
fs.MouseButton1Click:Connect(function()
    si=si%#SM+1 cfg.sm=SM[si].v fs.Text="Плавность: "..SM[si].n
end)
local TG={
    {n="ГОЛОВА",b="Head"},{n="ТЕЛО",b="UpperTorso"},
    {n="ЦЕНТР",b="HumanoidRootPart"}
} local gi=1
local ft=btn("Цель: ГОЛОВА",y) st(ft,true) step()
ft.MouseButton1Click:Connect(function()
    gi=gi%#TG+1 cfg.tgt=TG[gi].b ft.Text="Цель: "..TG[gi].n
end)

Scroll.CanvasSize=UDim2.new(0,0,0,y+10)

QB.MouseButton1Click:Connect(function()
    cfg.aim=1-cfg.aim
    QB.Text=cfg.aim==1 and "AIM\nON" or "AIM\nOFF"
    QB.BackgroundColor3=cfg.aim==1 and UI.on or UI.off
    QB.TextColor3=cfg.aim==1 and UI.td or UI.tl
    b8.Text=cfg.aim==1 and "Автоприцел: ВКЛ" or "Автоприцел: ВЫКЛ"
    st(b8,cfg.aim==1)
end)

local collapsed=false
CB.MouseButton1Click:Connect(function()
    collapsed=not collapsed
    M.Size=collapsed and UDim2.new(0,330,0,40) or UDim2.new(0,330,0,480)
    CB.Text=collapsed and "+" or "-"
    Scroll.Visible=not collapsed
end)

R.RenderStepped:Connect(function(dt)
    if cfg.fb==1 then
        L.Brightness=5 L.Ambient=Color3.fromRGB(220,220,220)
        L.OutdoorAmbient=Color3.fromRGB(220,220,220) L.FogEnd=1e6
        L.ClockTime=14 L.GlobalShadows=false
    end
    if cfg.sprint~=16 then applySpeed() end
    if cfg.aim==1 and cfg.circ==1 then
        FOV.Position=Vector2.new(C.ViewportSize.X/2,C.ViewportSize.Y/2)
        FOV.Radius=cfg.fov FOV.Visible=true
    else FOV.Visible=false end
    if cfg.aim==1 then
        local tg=aimT()
        if tg then
            local cp=C.CFrame.Position
            local ld=tg.Position-cp
            if ld.Magnitude>.1 then
                C.CFrame=C.CFrame:Lerp(CFrame.new(cp,cp+ld),
                    math.clamp(cfg.sm*dt*60,0,1))
            end
        end
    end
    if cfg.tr==1 then
        local o=cfg.trb==1
            and Vector2.new(C.ViewportSize.X/2,C.ViewportSize.Y)
            or  Vector2.new(C.ViewportSize.X/2,C.ViewportSize.Y/2)
        for _,p in ipairs(P:GetPlayers()) do
            if p~=LP then
                local l=gTr(p)
                local ch=p.Character
                local hp=ch and ch:FindFirstChild("HumanoidRootPart")
                local hm=ch and ch:FindFirstChildOfClass("Humanoid")
                if hp and hm and hm.Health>0 and en(p) then
                    local sp,on=C:WorldToViewportPoint(hp.Position)
                    if on and sp.Z>0 then
                        l.From=o l.To=Vector2.new(sp.X,sp.Y)
                        l.Color=col(p) l.Thickness=cfg.thick l.Visible=true
                    else l.Visible=false end
                else l.Visible=false end
            end
        end
    else
        for _,l in pairs(TR) do l.Visible=false end
    end
end)

U.InputBegan:Connect(function(i,g)
    if not g and i.KeyCode==Enum.KeyCode.RightShift then
        M.Visible=not M.Visible
    end
end)
P.PlayerRemoving:Connect(function(p)
    dESP(p) if TR[p] then TR[p]:Remove() TR[p]=nil end
end)
P.PlayerAdded:Connect(function(p)
    p.CharacterAdded:Connect(function(c)
        task.wait(.5) if cfg.esp==1 then aESP(p,c) end
    end)
end)
if cfg.esp==1 then refESP() end
LP.CharacterAdded:Connect(function(c) task.wait(1) applySpeed() end)
task.spawn(function()
    while task.wait(1) do
        if cfg.esp==1 then
            for _,p in ipairs(P:GetPlayers()) do
                if p~=LP and p.Character then aESP(p,p.Character) end
            end
        end
    end
end)
