-- \\
-- \\ TIME
-- \\
local ffi = require("ffi")
ffi.cdef[[
    typedef struct {
      unsigned short wYear;
      unsigned short wMonth;
      unsigned short wDayOfWeek;
      unsigned short wDay;
      unsigned short wHour;
      unsigned short wMinute;
      unsigned short wSecond;
      unsigned short wMilliseconds;
    } SYSTEMTIME, *LPSYSTEMTIME;
    void GetSystemTime(LPSYSTEMTIME lpSystemTime);
    void GetLocalTime(LPSYSTEMTIME lpSystemTime);
]]
-- \\
-- \\ END
-- \\

local colorwatermark = cheat.Color("Water color")
local alpha = cheat.SliderInt("Background alpha", 0, 255)
local font = g_Render.InitFont("Verdana",12)
local frame_rate = 0.0
local function get_abs_fps()
frame_rate = 0.9 * frame_rate + (1.0 - 0.9) * g_GlobalVars.absoluteframetime
return math.floor((1.0 / frame_rate) + 0.5)
end
local function get_latency()
local netchann_info = g_EngineClient.GetNetChannelInfo()
if netchann_info == nil then return "0" end
local latency = netchann_info:GetLatency(0)
return string.format("%1.f", math.max(0.0, latency) * 1000.0)
end
local textSize = 0
local function watermark()
  local screen = g_EngineClient.GetScreenSize()
  local fps = get_abs_fps()
  local ping = get_latency()
  local ticks = math.floor(1.0 / g_GlobalVars.interval_per_tick)
  local rightPadding = 20
  local var = screen.x - textSize - rightPadding
  local x = var - 10
  local y = 9
  local w = textSize + 20
  local h = 17
  local ip = ""
  if ( ping < "100" ) then
    g_Render.BoxFilled(Vector2.new(x+5,y+2),Vector2.new(x+textSize+15,h * 1.5 + 2), Color.new(0,0,0, alpha:GetInt()/255))
    g_Render.BoxFilled(Vector2.new(x+5,y),Vector2.new(x+textSize+15,h-6), colorwatermark:GetColor())
    local nexttext = "neverlose"
    g_Render.Text(nexttext, Vector2.new(var,12), Color.new(255,255,255), 12, font)
    local wide = g_Render.CalcTextSize(nexttext, 12, font)
    var = var + wide.x
    local username = string.lower(cheat.GetCheatUserName())
    nexttext = " | " .. username
    g_Render.Text(nexttext, Vector2.new(var,12), Color.new(255,255,255), 12,font)
    wide = g_Render.CalcTextSize(nexttext, 12,font)
    var = var + wide.x
    if g_EngineClient.GetNetChannelInfo() == nil then
      ip = "loopback"
    else
      nexttext = " | delay: ".. ping .."ms"
      g_Render.Text(nexttext, Vector2.new(var,12), Color.new(255,255,255), 12,font)
      wide = g_Render.CalcTextSize(nexttext, 12,font)
      var = var + wide.x
      nexttext = " | " .. ticks .. "tick"
      g_Render.Text(nexttext, Vector2.new(var,12), Color.new(255,255,255), 12,font)
      wide = g_Render.CalcTextSize(nexttext, 12,font)
      var = var + wide.x
    end
    local local_time = ffi.new("SYSTEMTIME")
    ffi.C.GetLocalTime(local_time)
    nexttext = " | " .. string.format('%02d:%02d:%02d', local_time.wHour, local_time.wMinute, local_time.wSecond)
    g_Render.Text(nexttext, Vector2.new(var,12), Color.new(255,255,255), 12,font)
    wide = g_Render.CalcTextSize(nexttext, 12,font)
    var = var + wide.x
    textSize = var - (screen.x - textSize - rightPadding)
  elseif ( ping >= "100" ) then
    g_Render.BoxFilled(Vector2.new(x+5,y+2),Vector2.new(x+textSize+15,h * 1.5 + 2), Color.new(0,0,0, alpha:GetInt()/255))
    g_Render.BoxFilled(Vector2.new(x+5,y),Vector2.new(x+textSize+15,h-6), colorwatermark:GetColor())
    local nexttext = "neverlose"
    g_Render.Text(nexttext, Vector2.new(var,12), Color.new(255,255,255), 12, font)
    local wide = g_Render.CalcTextSize(nexttext, 12, font)
    var = var + wide.x
    local username = string.lower(cheat.GetCheatUserName())
    nexttext = " | " .. username
    g_Render.Text(nexttext, Vector2.new(var,12), Color.new(255,255,255), 12,font)
    wide = g_Render.CalcTextSize(nexttext, 12,font)
    var = var + wide.x
    nexttext = " | delay: ".. ping .."ms"
    g_Render.Text(nexttext, Vector2.new(var,12), Color.new(255,255,255), 12,font)
    wide = g_Render.CalcTextSize(nexttext, 12,font)
    var = var + wide.x
    nexttext = " | " .. ticks .. "tick"
    g_Render.Text(nexttext, Vector2.new(var,12), Color.new(255,255,255), 12,font)
    wide = g_Render.CalcTextSize(nexttext, 12,font)
    var = var + wide.x
    local local_time = ffi.new("SYSTEMTIME")
    ffi.C.GetLocalTime(local_time)
    nexttext = " | " .. string.format('%02d:%02d:%02d', local_time.wHour, local_time.wMinute, local_time.wSecond)
    g_Render.Text(nexttext, Vector2.new(var,12), Color.new(255,255,255), 12,font)
    wide = g_Render.CalcTextSize(nexttext, 12,font)
    var = var + wide.x
    textSize = var - (screen.x - textSize - rightPadding)
  end
end
cheat.RegisterCallback("draw",watermark)
