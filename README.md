getgenv().Config = {
    Box = {
        Enable = true,
        Type = 'Full', -- Corner
        Font = 'SourceSans', -- Using default Roblox font
        Color = Color3.fromRGB(255, 255, 255),
        Outline = {
            Enable = true,
            Color = Color3.fromRGB(0, 0, 0)
        },
        Filled = {
            Enable = false,
            Gradient = {
                Enable = false,
                Color = {
                    Start = Color3.fromRGB(255, 255, 255),
                    End = Color3.fromRGB(0, 0, 0)
                },
                Rotation = {
                    Enable = true,
                    Auto = true
                },
                Transparency = 0.5
            }
        }
    },
    Text = {
        Enable = false,
        Position = 'Above', -- Options: 'Above' or 'Below'
        FontSize = 12, -- Font size for text labels
        Name = {
            Enable = false,
            Teamcheck = true,
            Color = Color3.fromRGB(255, 255, 255)
        },
        Studs = {
            Enable = false,
            Color = Color3.fromRGB(255, 255, 255)
        },
        Tool = {
            Enable = false,
            Color = Color3.fromRGB(255, 255, 255)
        }
    },
    Bars = {
        Enable = false,
        Health = {
            ShowOutline = false,
            Enable = false,
            Lerp = true,
            Color1 = Color3.fromRGB(0, 255, 0),
            Color2 = Color3.fromRGB(255, 255, 0),
            Color3 = Color3.fromRGB(255, 0, 0)
        },
        Armor = {
            ShowOutline = false,
            Enable = false,
            Lerp = true,
            Color1 = Color3.fromRGB(0, 0, 255),
            Color2 = Color3.fromRGB(135, 206, 235),
            Color3 = Color3.fromRGB(1, 0, 0)
        }
    },
    Aura = {
        Enable = false,
        Type = "12" -- Options: "1" to "20" (see below for descriptions)
    },
    Forcefield = {
        Enabled = false, -- Enable or disable the forcefield effect
        Color = Color3.fromRGB(0, 255, 0) -- Color of the forcefield
    },
    Distance = {
        Max = 1000 -- Maximum distance in studs
    }
}

if not LPH_OBFUSCATED then
    LPH_JIT_MAX = function(...) return (...) end
    LPH_NO_VIRTUALIZE = function(...) return (...) end
end

local Overlay = {}
local utility, connections, cache = {}, {}, {}
utility.funcs = {}

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- Wait for LocalPlayer to be valid
repeat wait() until Players.LocalPlayer
local player = Players.LocalPlayer

local HitEffectModule = {
    Locals = {
        HitEffect = {
            Type = {}
        }
    }
}

-- Aura Option 1: Skibidi RedRizz (Purple/Pink Swirls with Sparks)
local function createAura1(attachment)
    local HealingWave1 = Instance.new("ParticleEmitter")
    HealingWave1.Name = "Healing Wave 1"
    HealingWave1.Lifetime = NumberRange.new(1.5, 1.5)
    HealingWave1.SpreadAngle = Vector2.new(10, -10)
    HealingWave1.LockedToPart = true
    HealingWave1.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.2, 0.7), NumberSequenceKeypoint.new(1, 1)})
    HealingWave1.LightEmission = 0.4
    HealingWave1.Color = ColorSequence.new(Color3.fromRGB(234, 8, 255))
    HealingWave1.VelocitySpread = 10
    HealingWave1.Speed = NumberRange.new(3, 6)
    HealingWave1.Brightness = 10
    HealingWave1.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 3), NumberSequenceKeypoint.new(1, 0.5)})
    HealingWave1.Rate = 20
    HealingWave1.Texture = "rbxassetid://8047533775"
    HealingWave1.RotSpeed = NumberRange.new(200, 400)
    HealingWave1.Parent = attachment

    local Sparks = Instance.new("ParticleEmitter")
    Sparks.Name = "Sparks"
    Sparks.Lifetime = NumberRange.new(0.5, 2)
    Sparks.SpreadAngle = Vector2.new(180, -180)
    Sparks.LightEmission = 1
    Sparks.Color = ColorSequence.new(Color3.fromRGB(255, 21, 255))
    Sparks.VelocitySpread = 180
    Sparks.Speed = NumberRange.new(5, 15)
    Sparks.Brightness = 10
    Sparks.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.2, 0.4), NumberSequenceKeypoint.new(1, 0)})
    Sparks.Acceleration = Vector3.new(0, 3, 0)
    Sparks.Rate = 40
    Sparks.Texture = "rbxassetid://8611887361"
    Sparks.RotSpeed = NumberRange.new(-30, 30)
    Sparks.Parent = attachment

    return {HealingWave1, Sparks}
end

-- Aura Option 2: Celestial Twinkle (Starry Blue/Gold Flicker)
local function createAura2(attachment)
    local StarTwinkle = Instance.new("ParticleEmitter")
    StarTwinkle.Name = "Star Twinkle"
    StarTwinkle.Lifetime = NumberRange.new(1, 2)
    StarTwinkle.SpreadAngle = Vector2.new(360, -360)
    StarTwinkle.LockedToPart = true
    StarTwinkle.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.8), NumberSequenceKeypoint.new(0.5, 0), NumberSequenceKeypoint.new(1, 0.8)})
    StarTwinkle.LightEmission = 0.6
    StarTwinkle.Color = ColorSequence.new(Color3.fromRGB(0, 191, 255), Color3.fromRGB(255, 215, 0))
    StarTwinkle.VelocitySpread = 180
    StarTwinkle.Speed = NumberRange.new(1, 3)
    StarTwinkle.Brightness = 8
    StarTwinkle.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.1), NumberSequenceKeypoint.new(0.5, 0.3), NumberSequenceKeypoint.new(1, 0)})
    StarTwinkle.Rate = 30
    StarTwinkle.Texture = "rbxassetid://244681548"
    StarTwinkle.RotSpeed = NumberRange.new(-20, 20)
    StarTwinkle.Parent = attachment

    local GlowDust = Instance.new("ParticleEmitter")
    GlowDust.Name = "Glow Dust"
    GlowDust.Lifetime = NumberRange.new(2, 3)
    GlowDust.SpreadAngle = Vector2.new(15, -15)
    GlowDust.LockedToPart = true
    GlowDust.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.2, 0.5), NumberSequenceKeypoint.new(1, 1)})
    GlowDust.LightEmission = 0.5
    GlowDust.Color = ColorSequence.new(Color3.fromRGB(135, 206, 250))
    GlowDust.VelocitySpread = 10
    GlowDust.Speed = NumberRange.new(2, 4)
    GlowDust.Brightness = 7
    GlowDust.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(1, 0)})
    GlowDust.Rate = 10
    GlowDust.Texture = "rbxassetid://243098823"
    GlowDust.Parent = attachment

    return {StarTwinkle, GlowDust}
end

-- Aura Option 3: Fiery Burst (Red/Orange Explosive Flames)
local function createAura3(attachment)
    local FlameBurst = Instance.new("ParticleEmitter")
    FlameBurst.Name = "Flame Burst"
    FlameBurst.Lifetime = NumberRange.new(0.5, 1)
    FlameBurst.SpreadAngle = Vector2.new(90, -90)
    FlameBurst.LockedToPart = true
    FlameBurst.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.7, 0), NumberSequenceKeypoint.new(1, 1)})
    FlameBurst.LightEmission = 1.2
    FlameBurst.Color = ColorSequence.new(Color3.fromRGB(255, 69, 0), Color3.fromRGB(255, 165, 0))
    FlameBurst.VelocitySpread = 90
    FlameBurst.Speed = NumberRange.new(10, 20)
    FlameBurst.Brightness = 12
    FlameBurst.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 2), NumberSequenceKeypoint.new(0.5, 1), NumberSequenceKeypoint.new(1, 0)})
    FlameBurst.Rate = 15
    FlameBurst.Texture = "rbxassetid://241594769"
    FlameBurst.RotSpeed = NumberRange.new(50, 100)
    FlameBurst.Parent = attachment

    local EmberCloud = Instance.new("ParticleEmitter")
    EmberCloud.Name = "Ember Cloud"
    EmberCloud.Lifetime = NumberRange.new(1, 2)
    EmberCloud.SpreadAngle = Vector2.new(180, -180)
    EmberCloud.LightEmission = 0.9
    EmberCloud.Color = Color3.fromRGB(255, 140, 0)
    EmberCloud.VelocitySpread = 180
    EmberCloud.Speed = NumberRange.new(3, 6)
    EmberCloud.Brightness = 9
    EmberCloud.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.5), NumberSequenceKeypoint.new(1, 0)})
    EmberCloud.Acceleration = Vector3.new(0, 5, 0)
    EmberCloud.Rate = 20
    EmberCloud.Texture = "rbxassetid://243660652"
    EmberCloud.Parent = attachment

    return {FlameBurst, EmberCloud}
end

-- Aura Option 4: Mystic Swirl (Purple/Blue Spiraling Energy)
local function createAura4(attachment)
    local EnergySwirl = Instance.new("ParticleEmitter")
    EnergySwirl.Name = "Energy Swirl"
    EnergySwirl.Lifetime = NumberRange.new(2, 3)
    EnergySwirl.SpreadAngle = Vector2.new(5, -5)
    EnergySwirl.LockedToPart = true
    EnergySwirl.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.3, 0.3), NumberSequenceKeypoint.new(1, 1)})
    EnergySwirl.LightEmission = 0.7
    EnergySwirl.Color = ColorSequence.new(Color3.fromRGB(147, 112, 219), Color3.fromRGB(65, 105, 225))
    EnergySwirl.VelocitySpread = 5
    EnergySwirl.Speed = NumberRange.new(2, 4)
    EnergySwirl.Brightness = 8
    EnergySwirl.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 1.5), NumberSequenceKeypoint.new(1, 0.5)})
    EnergySwirl.Rate = 25
    EnergySwirl.Texture = "rbxassetid://296874871"
    EnergySwirl.RotSpeed = NumberRange.new(200, 400)
    EnergySwirl.Parent = attachment

    local MysticMist = Instance.new("ParticleEmitter")
    MysticMist.Name = "Mystic Mist"
    MysticMist.Lifetime = NumberRange.new(3, 4)
    MysticMist.SpreadAngle = Vector2.new(15, -15)
    MysticMist.LockedToPart = true
    MysticMist.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.5), NumberSequenceKeypoint.new(1, 0.5)})
    MysticMist.LightEmission = 0.6
    MysticMist.Color = ColorSequence.new(Color3.fromRGB(186, 85, 211))
    MysticMist.VelocitySpread = 15
    MysticMist.Speed = NumberRange.new(1, 2)
    MysticMist.Brightness = 6
    MysticMist.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 2), NumberSequenceKeypoint.new(1, 0)})
    MysticMist.Rate = 10
    MysticMist.Texture = "rbxassetid://243098823"
    MysticMist.Parent = attachment

    return {EnergySwirl, MysticMist}
end

-- Aura Option 5: Neon Flash (Green/Cyan Electric Pulses)
local function createAura5(attachment)
    local ElectricPulse = Instance.new("ParticleEmitter")
    ElectricPulse.Name = "Electric Pulse"
    ElectricPulse.Lifetime = NumberRange.new(0.5, 1)
    ElectricPulse.SpreadAngle = Vector2.new(20, -20)
    ElectricPulse.LockedToPart = true
    ElectricPulse.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.2, 0), NumberSequenceKeypoint.new(0.8, 0), NumberSequenceKeypoint.new(1, 1)})
    ElectricPulse.LightEmission = 1.1
    ElectricPulse.Color = ColorSequence.new(Color3.fromRGB(0, 255, 127), Color3.fromRGB(0, 255, 255))
    ElectricPulse.VelocitySpread = 20
    ElectricPulse.Speed = NumberRange.new(5, 10)
    ElectricPulse.Brightness = 11
    ElectricPulse.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 1.5), NumberSequenceKeypoint.new(1, 0)})
    ElectricPulse.Rate = 30
    ElectricPulse.Texture = "rbxassetid://296874871"
    ElectricPulse.RotSpeed = NumberRange.new(100, 200)
    ElectricPulse.Parent = attachment

    local NeonSparks = Instance.new("ParticleEmitter")
    NeonSparks.Name = "Neon Sparks"
    NeonSparks.Lifetime = NumberRange.new(0.3, 0.7)
    NeonSparks.SpreadAngle = Vector2.new(360, -360)
    NeonSparks.LightEmission = 1
    NeonSparks.Color = Color3.fromRGB(0, 255, 255)
    NeonSparks.VelocitySpread = 360
    NeonSparks.Speed = NumberRange.new(7, 12)
    NeonSparks.Brightness = 10
    NeonSparks.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.2), NumberSequenceKeypoint.new(1, 0)})
    NeonSparks.Acceleration = Vector3.new(0, 6, 0)
    NeonSparks.Rate = 40
    NeonSparks.Texture = "rbxassetid://243660652"
    NeonSparks.Parent = attachment

    return {ElectricPulse, NeonSparks}
end

-- Aura Option 6: Frosty Chill (Icy Blue with Snowflakes)
local function createAura6(attachment)
    local IceWave = Instance.new("ParticleEmitter")
    IceWave.Name = "Ice Wave"
    IceWave.Lifetime = NumberRange.new(2, 3)
    IceWave.SpreadAngle = Vector2.new(10, -10)
    IceWave.LockedToPart = true
    IceWave.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.3, 0.4), NumberSequenceKeypoint.new(1, 1)})
    IceWave.LightEmission = 0.5
    IceWave.Color = ColorSequence.new(Color3.fromRGB(173, 216, 230))
    IceWave.VelocitySpread = 10
    IceWave.Speed = NumberRange.new(2, 4)
    IceWave.Brightness = 6
    IceWave.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 1.5), NumberSequenceKeypoint.new(1, 0)})
    IceWave.Rate = 15
    IceWave.Texture = "rbxassetid://1561848370"
    IceWave.RotSpeed = NumberRange.new(30, 60)
    IceWave.Parent = attachment

    local Snowflakes = Instance.new("ParticleEmitter")
    Snowflakes.Name = "Snowflakes"
    Snowflakes.Lifetime = NumberRange.new(1.5, 2.5)
    Snowflakes.SpreadAngle = Vector2.new(180, -180)
    Snowflakes.LightEmission = 0.4
    Snowflakes.Color = Color3.fromRGB(240, 248, 255)
    Snowflakes.VelocitySpread = 180
    Snowflakes.Speed = NumberRange.new(1, 3)
    Snowflakes.Brightness = 5
    Snowflakes.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.3), NumberSequenceKeypoint.new(1, 0)})
    Snowflakes.Acceleration = Vector3.new(0, -2, 0)
    Snowflakes.Rate = 20
    Snowflakes.Texture = "rbxassetid://1561848370"
    Snowflakes.RotSpeed = NumberRange.new(-10, 10)
    Snowflakes.Parent = attachment

    return {IceWave, Snowflakes}
end

-- Aura Option 7: Golden Radiance (Warm Gold with Rays)
local function createAura7(attachment)
    local RadianceRay = Instance.new("ParticleEmitter")
    RadianceRay.Name = "Radiance Ray"
    RadianceRay.Lifetime = NumberRange.new(1.5, 2)
    RadianceRay.SpreadAngle = Vector2.new(5, -5)
    RadianceRay.LockedToPart = true
    RadianceRay.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.2, 0.2), NumberSequenceKeypoint.new(1, 1)})
    RadianceRay.LightEmission = 0.8
    RadianceRay.Color = ColorSequence.new(Color3.fromRGB(255, 215, 0))
    RadianceRay.VelocitySpread = 5
    RadianceRay.Speed = NumberRange.new(3, 5)
    RadianceRay.Brightness = 9
    RadianceRay.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 2), NumberSequenceKeypoint.new(1, 0)})
    RadianceRay.Rate = 20
    RadianceRay.Texture = "rbxassetid://194799085"
    RadianceRay.RotSpeed = NumberRange.new(50, 100)
    RadianceRay.Parent = attachment

    local GlowSpecks = Instance.new("ParticleEmitter")
    GlowSpecks.Name = "Glow Specks"
    GlowSpecks.Lifetime = NumberRange.new(1, 2)
    GlowSpecks.SpreadAngle = Vector2.new(360, -360)
    GlowSpecks.LightEmission = 0.7
    GlowSpecks.Color = Color3.fromRGB(255, 223, 0)
    GlowSpecks.VelocitySpread = 360
    GlowSpecks.Speed = NumberRange.new(2, 4)
    GlowSpecks.Brightness = 8
    GlowSpecks.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.2), NumberSequenceKeypoint.new(1, 0)})
    GlowSpecks.Rate = 25
    GlowSpecks.Texture = "rbxassetid://194799085"
    GlowSpecks.Parent = attachment

    return {RadianceRay, GlowSpecks}
end

-- Aura Option 8: Shadow Pulse (Dark Gray with Fading Rings)
local function createAura8(attachment)
    local ShadowRing = Instance.new("ParticleEmitter")
    ShadowRing.Name = "Shadow Ring"
    ShadowRing.Lifetime = NumberRange.new(2, 3)
    ShadowRing.SpreadAngle = Vector2.new(0, 0)
    ShadowRing.LockedToPart = true
    ShadowRing.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.5, 0.5), NumberSequenceKeypoint.new(1, 1)})
    ShadowRing.LightEmission = 0.3
    ShadowRing.Color = ColorSequence.new(Color3.fromRGB(105, 105, 105))
    ShadowRing.VelocitySpread = 0
    ShadowRing.Speed = NumberRange.new(2, 3)
    ShadowRing.Brightness = 4
    ShadowRing.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 3), NumberSequenceKeypoint.new(1, 0)})
    ShadowRing.Rate = 5
    ShadowRing.Texture = "rbxassetid://243098823"
    ShadowRing.Parent = attachment

    local ShadowMist = Instance.new("ParticleEmitter")
    ShadowMist.Name = "Shadow Mist"
    ShadowMist.Lifetime = NumberRange.new(3, 4)
    ShadowMist.SpreadAngle = Vector2.new(15, -15)
    ShadowMist.LockedToPart = true
    ShadowMist.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.7), NumberSequenceKeypoint.new(1, 0.7)})
    ShadowMist.LightEmission = 0.2
    ShadowMist.Color = ColorSequence.new(Color3.fromRGB(75, 75, 75))
    ShadowMist.VelocitySpread = 15
    ShadowMist.Speed = NumberRange.new(1, 2)
    ShadowMist.Brightness = 3
    ShadowMist.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 2), NumberSequenceKeypoint.new(1, 0)})
    ShadowMist.Rate = 10
    ShadowMist.Texture = "rbxassetid://243098823"
    ShadowMist.Parent = attachment

    return {ShadowRing, ShadowMist}
end

-- Aura Option 9: Emerald Bloom (Green with Petal-like Particles)
local function createAura9(attachment)
    local PetalWave = Instance.new("ParticleEmitter")
    PetalWave.Name = "Petal Wave"
    PetalWave.Lifetime = NumberRange.new(2, 3)
    PetalWave.SpreadAngle = Vector2.new(20, -20)
    PetalWave.LockedToPart = true
    PetalWave.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.3, 0), NumberSequenceKeypoint.new(1, 1)})
    PetalWave.LightEmission = 0.6
    PetalWave.Color = ColorSequence.new(Color3.fromRGB(0, 128, 0))
    PetalWave.VelocitySpread = 20
    PetalWave.Speed = NumberRange.new(2, 4)
    PetalWave.Brightness = 7
    PetalWave.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(1, 0)})
    PetalWave.Rate = 15
    PetalWave.Texture = "rbxassetid://157089063"
    PetalWave.RotSpeed = NumberRange.new(30, 60)
    PetalWave.Parent = attachment

    local Pollen = Instance.new("ParticleEmitter")
    Pollen.Name = "Pollen"
    Pollen.Lifetime = NumberRange.new(1.5, 2.5)
    Pollen.SpreadAngle = Vector2.new(180, -180)
    Pollen.LightEmission = 0.5
    Pollen.Color = Color3.fromRGB(144, 238, 144)
    Pollen.VelocitySpread = 180
    Pollen.Speed = NumberRange.new(1, 3)
    Pollen.Brightness = 6
    Pollen.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.2), NumberSequenceKeypoint.new(1, 0)})
    Pollen.Acceleration = Vector3.new(0, -1, 0)
    Pollen.Rate = 20
    Pollen.Texture = "rbxassetid://157089063"
    Pollen.RotSpeed = NumberRange.new(-10, 10)
    Pollen.Parent = attachment

    return {PetalWave, Pollen}
end

-- Aura Option 10: Ruby Flare (Red with Glowing Shards)
local function createAura10(attachment)
    local FlareWave = Instance.new("ParticleEmitter")
    FlareWave.Name = "Flare Wave"
    FlareWave.Lifetime = NumberRange.new(1, 2)
    FlareWave.SpreadAngle = Vector2.new(10, -10)
    FlareWave.LockedToPart = true
    FlareWave.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.2, 0.2), NumberSequenceKeypoint.new(1, 1)})
    FlareWave.LightEmission = 0.9
    FlareWave.Color = ColorSequence.new(Color3.fromRGB(255, 20, 147))
    FlareWave.VelocitySpread = 10
    FlareWave.Speed = NumberRange.new(3, 6)
    FlareWave.Brightness = 10
    FlareWave.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 2), NumberSequenceKeypoint.new(1, 0)})
    FlareWave.Rate = 20
    FlareWave.Texture = "rbxassetid://241594769"
    FlareWave.RotSpeed = NumberRange.new(50, 100)
    FlareWave.Parent = attachment

    local ShardGlow = Instance.new("ParticleEmitter")
    ShardGlow.Name = "Shard Glow"
    ShardGlow.Lifetime = NumberRange.new(0.5, 1)
    ShardGlow.SpreadAngle = Vector2.new(360, -360)
    ShardGlow.LightEmission = 0.8
    ShardGlow.Color = Color3.fromRGB(255, 105, 180)
    ShardGlow.VelocitySpread = 360
    ShardGlow.Speed = NumberRange.new(4, 8)
    ShardGlow.Brightness = 9
    ShardGlow.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.3), NumberSequenceKeypoint.new(1, 0)})
    ShardGlow.Acceleration = Vector3.new(0, 3, 0)
    ShardGlow.Rate = 25
    ShardGlow.Texture = "rbxassetid://243660652"
    ShardGlow.Parent = attachment

    return {FlareWave, ShardGlow}
end

-- Aura Option 11: Sapphire Cascade (Blue with Water Droplets)
local function createAura11(attachment)
    local WaterDrop = Instance.new("ParticleEmitter")
    WaterDrop.Name = "Water Drop"
    WaterDrop.Lifetime = NumberRange.new(2, 3)
    WaterDrop.SpreadAngle = Vector2.new(15, -15)
    WaterDrop.LockedToPart = true
    WaterDrop.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.3, 0), NumberSequenceKeypoint.new(1, 1)})
    WaterDrop.LightEmission = 0.5
    WaterDrop.Color = ColorSequence.new(Color3.fromRGB(0, 191, 255))
    WaterDrop.VelocitySpread = 15
    WaterDrop.Speed = NumberRange.new(2, 4)
    WaterDrop.Brightness = 6
    WaterDrop.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.5), NumberSequenceKeypoint.new(1, 0)})
    WaterDrop.Rate = 20
    WaterDrop.Texture = "rbxassetid://1561848370"
    WaterDrop.Acceleration = Vector3.new(0, -3, 0)
    WaterDrop.Parent = attachment

    local Ripple = Instance.new("ParticleEmitter")
    Ripple.Name = "Ripple"
    Ripple.Lifetime = NumberRange.new(1.5, 2.5)
    Ripple.SpreadAngle = Vector2.new(0, 0)
    Ripple.LockedToPart = true
    Ripple.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0), NumberSequenceKeypoint.new(0.5, 0.5), NumberSequenceKeypoint.new(1, 1)})
    Ripple.LightEmission = 0.4
    Ripple.Color = Color3.fromRGB(135, 206, 250)
    Ripple.VelocitySpread = 0
    Ripple.Speed = NumberRange.new(1, 2)
    Ripple.Brightness = 5
    Ripple.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 2), NumberSequenceKeypoint.new(1, 0)})
    Ripple.Rate = 10
    Ripple.Texture = "rbxassetid://243098823"
    Ripple.Parent = attachment

    return {WaterDrop, Ripple}
end

-- Aura Option 12: Amethyst Veil (Purple with Floating Crystals)
local function createAura12(attachment)
    local CrystalFloat = Instance.new("ParticleEmitter")
    CrystalFloat.Name = "Crystal Float"
    CrystalFloat.Lifetime = NumberRange.new(2, 3)
    CrystalFloat.SpreadAngle = Vector2.new(180, -180)
    CrystalFloat.LockedToPart = true
    CrystalFloat.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.5), NumberSequenceKeypoint.new(1, 0.5)})
    CrystalFloat.LightEmission = 0.7
    CrystalFloat.Color = ColorSequence.new(Color3.fromRGB(153, 50, 204))
    CrystalFloat.VelocitySpread = 180
    CrystalFloat.Speed = NumberRange.new(1, 2)
    CrystalFloat.Brightness = 8
    CrystalFloat.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.4), NumberSequenceKeypoint.new(1, 0)})
    CrystalFloat.Acceleration = Vector3.new(0, 1, 0)
    CrystalFloat.Rate = 15
    CrystalFloat.Texture = "rbxassetid://296874871"
    CrystalFloat.RotSpeed = NumberRange.new(20, 40)
    CrystalFloat.Parent = attachment

    local VeilMist = Instance.new("ParticleEmitter")
    VeilMist.Name = "Veil Mist"
    VeilMist.Lifetime = NumberRange.new(3, 4)
    VeilMist.SpreadAngle = Vector2.new(15, -15)
    VeilMist.LockedToPart = true
    VeilMist.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.6), NumberSequenceKeypoint.new(1, 0.6)})
    VeilMist.LightEmission = 0.6
    VeilMist.Color = ColorSequence.new(Color3.fromRGB(221, 160, 221))
    VeilMist.VelocitySpread = 15
    VeilMist.Speed = NumberRange.new(1, 2)
    VeilMist.Brightness = 7
    VeilMist.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 1.5), NumberSequenceKeypoint.new(1, 0)})
    VeilMist.Rate = 10
    VeilMist.Texture = "rbxassetid://243098823"
    VeilMist.Parent = attachment

    return {CrystalFloat, VeilMist}
end

-- Aura Option 13: Solar Flare (Yellow/Orange with Sunburst)
local function createAura13(attachment)
    local Sunburst = Instance.new("ParticleEmitter")
    Sunburst.Name = "Sunburst"
    Sunburst.Lifetime = NumberRange.new(1, 1.5)
    Sunburst.SpreadAngle = Vector2.new(5, -5)
    Sunburst.LockedToPart = true
    Sunburst.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.2, 0), NumberSequenceKeypoint.new(1, 1)})
    Sunburst.LightEmission = 1
    Sunburst.Color = ColorSequence.new(Color3.fromRGB(255, 215, 0), Color3.fromRGB(255, 165, 0))
    Sunburst.VelocitySpread = 5
    Sunburst.Speed = NumberRange.new(4, 7)
    Sunburst.Brightness = 11
    Sunburst.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 2.5), NumberSequenceKeypoint.new(1, 0)})
    Sunburst.Rate = 20
    Sunburst.Texture = "rbxassetid://194799085"
    Sunburst.RotSpeed = NumberRange.new(50, 100)
    Sunburst.Parent = attachment

    local SolarSpark = Instance.new("ParticleEmitter")
    SolarSpark.Name = "Solar Spark"
    SolarSpark.Lifetime = NumberRange.new(0.5, 1)
    SolarSpark.SpreadAngle = Vector2.new(360, -360)
    SolarSpark.LightEmission = 0.9
    SolarSpark.Color = Color3.fromRGB(255, 223, 0)
    SolarSpark.VelocitySpread = 360
    SolarSpark.Speed = NumberRange.new(5, 10)
    SolarSpark.Brightness = 10
    SolarSpark.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.3), NumberSequenceKeypoint.new(1, 0)})
    SolarSpark.Acceleration = Vector3.new(0, 4, 0)
    SolarSpark.Rate = 25
    SolarSpark.Texture = "rbxassetid://243660652"
    SolarSpark.Parent = attachment

    return {Sunburst, SolarSpark}
end

-- Aura Option 14: Lunar Glow (Silver with Crescent Waves)
local function createAura14(attachment)
    local CrescentWave = Instance.new("ParticleEmitter")
    CrescentWave.Name = "Crescent Wave"
    CrescentWave.Lifetime = NumberRange.new(2, 2.5)
    CrescentWave.SpreadAngle = Vector2.new(10, -10)
    CrescentWave.LockedToPart = true
    CrescentWave.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.3, 0.3), NumberSequenceKeypoint.new(1, 1)})
    CrescentWave.LightEmission = 0.6
    CrescentWave.Color = ColorSequence.new(Color3.fromRGB(192, 192, 192))
    CrescentWave.VelocitySpread = 10
    CrescentWave.Speed = NumberRange.new(2, 4)
    CrescentWave.Brightness = 7
    CrescentWave.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 1.5), NumberSequenceKeypoint.new(1, 0)})
    CrescentWave.Rate = 15
    CrescentWave.Texture = "rbxassetid://194799085"
    CrescentWave.RotSpeed = NumberRange.new(30, 60)
    CrescentWave.Parent = attachment

    local LunarDust = Instance.new("ParticleEmitter")
    LunarDust.Name = "Lunar Dust"
    LunarDust.Lifetime = NumberRange.new(1.5, 2)
    LunarDust.SpreadAngle = Vector2.new(180, -180)
    LunarDust.LightEmission = 0.5
    LunarDust.Color = Color3.fromRGB(211, 211, 211)
    LunarDust.VelocitySpread = 180
    LunarDust.Speed = NumberRange.new(1, 3)
    LunarDust.Brightness = 6
    LunarDust.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.2), NumberSequenceKeypoint.new(1, 0)})
    LunarDust.Acceleration = Vector3.new(0, -1, 0)
    LunarDust.Rate = 20
    LunarDust.Texture = "rbxassetid://244681548"
    LunarDust.RotSpeed = NumberRange.new(-10, 10)
    LunarDust.Parent = attachment

    return {CrescentWave, LunarDust}
end

-- Aura Option 15: Coral Reef (Pink/Orange with Bubble Flow)
local function createAura15(attachment)
    local BubbleFlow = Instance.new("ParticleEmitter")
    BubbleFlow.Name = "Bubble Flow"
    BubbleFlow.Lifetime = NumberRange.new(2, 3)
    BubbleFlow.SpreadAngle = Vector2.new(15, -15)
    BubbleFlow.LockedToPart = true
    BubbleFlow.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.3, 0), NumberSequenceKeypoint.new(1, 1)})
    BubbleFlow.LightEmission = 0.5
    BubbleFlow.Color = ColorSequence.new(Color3.fromRGB(255, 127, 80))
    BubbleFlow.VelocitySpread = 15
    BubbleFlow.Speed = NumberRange.new(2, 4)
    BubbleFlow.Brightness = 6
    BubbleFlow.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.5), NumberSequenceKeypoint.new(1, 0)})
    BubbleFlow.Rate = 20
    BubbleFlow.Texture = "rbxassetid://1561848370"
    BubbleFlow.Acceleration = Vector3.new(0, 2, 0)
    BubbleFlow.Parent = attachment

    local CoralGlow = Instance.new("ParticleEmitter")
    CoralGlow.Name = "Coral Glow"
    CoralGlow.Lifetime = NumberRange.new(1.5, 2.5)
    CoralGlow.SpreadAngle = Vector2.new(10, -10)
    CoralGlow.LockedToPart = true
    CoralGlow.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.2, 0.4), NumberSequenceKeypoint.new(1, 1)})
    CoralGlow.LightEmission = 0.6
    CoralGlow.Color = ColorSequence.new(Color3.fromRGB(255, 99, 71))
    CoralGlow.VelocitySpread = 10
    CoralGlow.Speed = NumberRange.new(1, 3)
    CoralGlow.Brightness = 7
    CoralGlow.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(1, 0)})
    CoralGlow.Rate = 15
    CoralGlow.Texture = "rbxassetid://243098823"
    CoralGlow.Parent = attachment

    return {BubbleFlow, CoralGlow}
end

-- Aura Option 16: Obsidian Shard (Black with Sharp Fragments)
local function createAura16(attachment)
    local ShardWave = Instance.new("ParticleEmitter")
    ShardWave.Name = "Shard Wave"
    ShardWave.Lifetime = NumberRange.new(1, 2)
    ShardWave.SpreadAngle = Vector2.new(20, -20)
    ShardWave.LockedToPart = true
    ShardWave.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.5), NumberSequenceKeypoint.new(1, 0.5)})
    ShardWave.LightEmission = 0.3
    ShardWave.Color = ColorSequence.new(Color3.fromRGB(47, 47, 47))
    ShardWave.VelocitySpread = 20
    ShardWave.Speed = NumberRange.new(3, 6)
    ShardWave.Brightness = 4
    ShardWave.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.5), NumberSequenceKeypoint.new(1, 0)})
    ShardWave.Rate = 20
    ShardWave.Texture = "rbxassetid://243660652"
    ShardWave.RotSpeed = NumberRange.new(40, 80)
    ShardWave.Parent = attachment

    local ObsidianDust = Instance.new("ParticleEmitter")
    ObsidianDust.Name = "Obsidian Dust"
    ObsidianDust.Lifetime = NumberRange.new(1.5, 2.5)
    ObsidianDust.SpreadAngle = Vector2.new(180, -180)
    ObsidianDust.LightEmission = 0.2
    ObsidianDust.Color = Color3.fromRGB(25, 25, 25)
    ObsidianDust.VelocitySpread = 180
    ObsidianDust.Speed = NumberRange.new(2, 4)
    ObsidianDust.Brightness = 3
    ObsidianDust.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.2), NumberSequenceKeypoint.new(1, 0)})
    ObsidianDust.Acceleration = Vector3.new(0, 2, 0)
    ObsidianDust.Rate = 25
    ObsidianDust.Texture = "rbxassetid://243660652"
    ObsidianDust.Parent = attachment

    return {ShardWave, ObsidianDust}
end

-- Aura Option 17: Topaz Spark (Yellow with Glittering Dust)
local function createAura17(attachment)
    local SparkWave = Instance.new("ParticleEmitter")
    SparkWave.Name = "Spark Wave"
    SparkWave.Lifetime = NumberRange.new(1.5, 2)
    SparkWave.SpreadAngle = Vector2.new(10, -10)
    SparkWave.LockedToPart = true
    SparkWave.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.2, 0.3), NumberSequenceKeypoint.new(1, 1)})
    SparkWave.LightEmission = 0.8
    SparkWave.Color = ColorSequence.new(Color3.fromRGB(255, 223, 0))
    SparkWave.VelocitySpread = 10
    SparkWave.Speed = NumberRange.new(3, 5)
    SparkWave.Brightness = 9
    SparkWave.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 1.5), NumberSequenceKeypoint.new(1, 0)})
    SparkWave.Rate = 20
    SparkWave.Texture = "rbxassetid://194799085"
    SparkWave.RotSpeed = NumberRange.new(50, 100)
    SparkWave.Parent = attachment

    local GlitterDust = Instance.new("ParticleEmitter")
    GlitterDust.Name = "Glitter Dust"
    GlitterDust.Lifetime = NumberRange.new(1, 2)
    GlitterDust.SpreadAngle = Vector2.new(360, -360)
    GlitterDust.LightEmission = 0.7
    GlitterDust.Color = Color3.fromRGB(255, 215, 0)
    GlitterDust.VelocitySpread = 360
    GlitterDust.Speed = NumberRange.new(2, 4)
    GlitterDust.Brightness = 8
    GlitterDust.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.1), NumberSequenceKeypoint.new(1, 0)})
    GlitterDust.Rate = 30
    GlitterDust.Texture = "rbxassetid://244681548"
    GlitterDust.Parent = attachment

    return {SparkWave, GlitterDust}
end

-- Aura Option 18: Aqua Swirl (Cyan with Spiraling Waves)
local function createAura18(attachment)
    local AquaWave = Instance.new("ParticleEmitter")
    AquaWave.Name = "Aqua Wave"
    AquaWave.Lifetime = NumberRange.new(2, 2.5)
    AquaWave.SpreadAngle = Vector2.new(5, -5)
    AquaWave.LockedToPart = true
    AquaWave.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.3, 0.2), NumberSequenceKeypoint.new(1, 1)})
    AquaWave.LightEmission = 0.6
    AquaWave.Color = ColorSequence.new(Color3.fromRGB(0, 255, 255))
    AquaWave.VelocitySpread = 5
    AquaWave.Speed = NumberRange.new(2, 4)
    AquaWave.Brightness = 7
    AquaWave.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 2), NumberSequenceKeypoint.new(1, 0)})
    AquaWave.Rate = 15
    AquaWave.Texture = "rbxassetid://243098823"
    AquaWave.RotSpeed = NumberRange.new(150, 300)
    AquaWave.Parent = attachment

    local SwirlMist = Instance.new("ParticleEmitter")
    SwirlMist.Name = "Swirl Mist"
    SwirlMist.Lifetime = NumberRange.new(2.5, 3.5)
    SwirlMist.SpreadAngle = Vector2.new(10, -10)
    SwirlMist.LockedToPart = true
    SwirlMist.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.7), NumberSequenceKeypoint.new(1, 0.7)})
    SwirlMist.LightEmission = 0.5
    SwirlMist.Color = Color3.fromRGB(0, 191, 255)
    SwirlMist.VelocitySpread = 10
    SwirlMist.Speed = NumberRange.new(1, 2)
    SwirlMist.Brightness = 6
    SwirlMist.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 1.5), NumberSequenceKeypoint.new(1, 0)})
    SwirlMist.Rate = 10
    SwirlMist.Texture = "rbxassetid://243098823"
    SwirlMist.Parent = attachment

    return {AquaWave, SwirlMist}
end

-- Aura Option 19: Crimson Vortex (Red with Twisting Flames)
local function createAura19(attachment)
    local FlameVortex = Instance.new("ParticleEmitter")
    FlameVortex.Name = "Flame Vortex"
    FlameVortex.Lifetime = NumberRange.new(1.5, 2)
    FlameVortex.SpreadAngle = Vector2.new(10, -10)
    FlameVortex.LockedToPart = true
    FlameVortex.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(0.2, 0), NumberSequenceKeypoint.new(1, 1)})
    FlameVortex.LightEmission = 1
    FlameVortex.Color = ColorSequence.new(Color3.fromRGB(255, 0, 0))
    FlameVortex.VelocitySpread = 10
    FlameVortex.Speed = NumberRange.new(4, 7)
    FlameVortex.Brightness = 11
    FlameVortex.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 1.5), NumberSequenceKeypoint.new(1, 0)})
    FlameVortex.Rate = 20
    FlameVortex.Texture = "rbxassetid://241594769"
    FlameVortex.RotSpeed = NumberRange.new(100, 200)
    FlameVortex.Parent = attachment

    local TwistedEmbers = Instance.new("ParticleEmitter")
    TwistedEmbers.Name = "Twisted Embers"
    TwistedEmbers.Lifetime = NumberRange.new(0.5, 1)
    TwistedEmbers.SpreadAngle = Vector2.new(360, -360)
    TwistedEmbers.LightEmission = 0.9
    TwistedEmbers.Color = Color3.fromRGB(255, 69, 0)
    TwistedEmbers.VelocitySpread = 360
    TwistedEmbers.Speed = NumberRange.new(3, 6)
    TwistedEmbers.Brightness = 10
    TwistedEmbers.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.2), NumberSequenceKeypoint.new(1, 0)})
    TwistedEmbers.Acceleration = Vector3.new(0, 3, 0)
    TwistedEmbers.Rate = 25
    TwistedEmbers.Texture = "rbxassetid://243660652"
    TwistedEmbers.RotSpeed = NumberRange.new(-50, 50)
    TwistedEmbers.Parent = attachment

    return {FlameVortex, TwistedEmbers}
end

-- Aura Option 20: Jade Mist (Green with Ethereal Fog)
local function createAura20(attachment)
    local MistWave = Instance.new("ParticleEmitter")
    MistWave.Name = "Mist Wave"
    MistWave.Lifetime = NumberRange.new(3, 4)
    MistWave.SpreadAngle = Vector2.new(15, -15)
    MistWave.LockedToPart = true
    MistWave.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.7), NumberSequenceKeypoint.new(1, 0.7)})
    MistWave.LightEmission = 0.4
    MistWave.Color = ColorSequence.new(Color3.fromRGB(0, 128, 0))
    MistWave.VelocitySpread = 15
    MistWave.Speed = NumberRange.new(1, 2)
    MistWave.Brightness = 5
    MistWave.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 2), NumberSequenceKeypoint.new(1, 0)})
    MistWave.Rate = 10
    MistWave.Texture = "rbxassetid://243098823"
    MistWave.Parent = attachment

    local JadeGlow = Instance.new("ParticleEmitter")
    JadeGlow.Name = "Jade Glow"
    JadeGlow.Lifetime = NumberRange.new(2, 3)
    JadeGlow.SpreadAngle = Vector2.new(10, -10)
    JadeGlow.LockedToPart = true
    JadeGlow.Transparency = NumberSequence.new({NumberSequenceKeypoint.new(0, 0.8), NumberSequenceKeypoint.new(0.5, 0.2), NumberSequenceKeypoint.new(1, 0.8)})
    JadeGlow.LightEmission = 0.6
    JadeGlow.Color = ColorSequence.new(Color3.fromRGB(144, 238, 144))
    JadeGlow.VelocitySpread = 10
    JadeGlow.Speed = NumberRange.new(2, 3)
    JadeGlow.Brightness = 6
    JadeGlow.Size = NumberSequence.new({NumberSequenceKeypoint.new(0, 1), NumberSequenceKeypoint.new(1, 0)})
    JadeGlow.Rate = 15
    JadeGlow.Texture = "rbxassetid://157089063"
    JadeGlow.Parent = attachment

    return {MistWave, JadeGlow}
end

-- Initialize aura based on type
local Attachment = Instance.new("Attachment")
local currentParticles = {}
local function initializeAura(type)
    for _, particle in pairs(currentParticles) do
        if particle and not particle.Parent then
            particle:Destroy()
        end
    end
    currentParticles = {}
    if type == "1" then
        currentParticles = createAura1(Attachment)
    elseif type == "2" then
        currentParticles = createAura2(Attachment)
    elseif type == "3" then
        currentParticles = createAura3(Attachment)
    elseif type == "4" then
        currentParticles = createAura4(Attachment)
    elseif type == "5" then
        currentParticles = createAura5(Attachment)
    elseif type == "6" then
        currentParticles = createAura6(Attachment)
    elseif type == "7" then
        currentParticles = createAura7(Attachment)
    elseif type == "8" then
        currentParticles = createAura8(Attachment)
    elseif type == "9" then
        currentParticles = createAura9(Attachment)
    elseif type == "10" then
        currentParticles = createAura10(Attachment)
    elseif type == "11" then
        currentParticles = createAura11(Attachment)
    elseif type == "12" then
        currentParticles = createAura12(Attachment)
    elseif type == "13" then
        currentParticles = createAura13(Attachment)
    elseif type == "14" then
        createAura14(Attachment)
    elseif type == "15" then
        createAura15(Attachment)
    elseif type == "16" then
        createAura16(Attachment)
    elseif type == "17" then
        createAura17(Attachment)
    elseif type == "18" then
        createAura18(Attachment)
    elseif type == "19" then
        createAura19(Attachment)
    elseif type == "20" then
        createAura20(Attachment)
    end
end

local function spawnAura()
    if not player or not player.Character or not player.Character:FindFirstChild("LowerTorso") then return end
    Attachment.Parent = player.Character.LowerTorso
    initializeAura(Config.Aura.Type)
    for _, particle in pairs(currentParticles) do
        if particle then
            particle.Enabled = Config.Aura.Enable
        end
    end
end

-- Forcefield Functions
local FORCEFIELD_MATERIAL = Enum.Material.ForceField
local DEFAULT_MATERIAL = Enum.Material.Plastic
local DEFAULT_COLOR = Color3.fromRGB(255, 255, 255)
local lastUpdateTime = 0
local updateInterval = 0.1

local function customizeCharacter(character, newColor)
    if not character then return end
    for _, part in ipairs(character:GetDescendants()) do
        if part:IsA("BasePart") and not part:FindFirstChild("ParticleEmitter") then
            if Config.Forcefield.Enabled then
                part.Color = newColor
                part.Material = FORCEFIELD_MATERIAL
            else
                part.Material = DEFAULT_MATERIAL
                part.Color = DEFAULT_COLOR
            end
        end
    end
end

local function onCharacterAdded(character)
    if not character then return end
    if Config.Forcefield.Enabled then
        customizeCharacter(character, Config.Forcefield.Color)
    end
end

-- Create a template BillboardGui for text ESP
local function create_billboard_gui()
    local gui = Instance.new("BillboardGui")
    gui.Name = "CrackedESP"
    gui.ResetOnSpawn = false
    gui.AlwaysOnTop = true
    gui.LightInfluence = 0
    gui.Size = UDim2.new(1.75, 0, 2, 0)
    gui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    gui.StudsOffset = Vector3.new(0, Config.Text.Position == 'Above' and 3 or -3, 0.5)

    local function create_text_label(name, offsetY)
        local label = Instance.new("TextLabel")
        label.Name = name
        label.Parent = gui
        label.Size = UDim2.new(1, 0, 0, Config.Text.FontSize)
        label.Position = UDim2.new(0, 0, 0, offsetY)
        label.BackgroundTransparency = 1
        label.TextColor3 = Color3.fromRGB(255, 255, 255)
        label.TextStrokeTransparency = 0
        label.TextScaled = false
        label.TextSize = Config.Text.FontSize
        label.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
        label.Font = Enum.Font[Config.Box.Font] or Enum.Font.SourceSans
        label.TextXAlignment = Enum.TextXAlignment.Center
        return label
    end

    local nameLabel = create_text_label("Name", 0)
    local studsLabel = create_text_label("Studs", Config.Text.FontSize)
    local toolLabel = create_text_label("Tool", Config.Text.FontSize * 2)

    return gui, nameLabel, studsLabel, toolLabel
end

utility.funcs.render = LPH_NO_VIRTUALIZE(function(player)
    if not player or not player.Parent then return end
    cache[player] = cache[player] or {}
    cache[player].Box = {}
    cache[player].Bars = {}
    cache[player].Text = {}

    cache[player].Box.Full = {
        Square = Drawing.new("Square"),
        Inline = Drawing.new("Square"),
        Outline = Drawing.new("Square"),
        Filled = Instance.new('Frame', Instance.new('ScreenGui', game.CoreGui))
    }

    if Config.Box.Filled.Gradient.Enable then
        local gradient = Instance.new("UIGradient")
        gradient.Name = "Gradient"
        gradient.Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, Config.Box.Filled.Gradient.Color.Start),
            ColorSequenceKeypoint.new(1, Config.Box.Filled.Gradient.Color.End)
        })
        gradient.Rotation = 90
        gradient.Parent = cache[player].Box.Full.Filled
    end

    local billboardGui, nameLabel, studsLabel, toolLabel = create_billboard_gui()
    cache[player].Text.BillboardGui = billboardGui
    cache[player].Text.Name = nameLabel
    cache[player].Text.Studs = studsLabel
    cache[player].Text.Tool = toolLabel

    local armorGui = Instance.new("ScreenGui")
    armorGui.Name = player.Name .. "_ArmorBar"
    armorGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    armorGui.Parent = game.CoreGui

    local armorOutline = Instance.new("Frame")
    armorOutline.BackgroundColor3 = Color3.new(0, 0, 0)
    armorOutline.BorderSizePixel = 0
    armorOutline.Name = "Outline"
    armorOutline.Parent = armorGui

    local armorFill = Instance.new("Frame")
    armorFill.BackgroundTransparency = 0
    armorFill.BorderSizePixel = 0
    armorFill.Name = "Fill"
    armorFill.Parent = armorOutline

    local armorGradient = Instance.new("UIGradient", armorFill)
    armorGradient.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, Config.Bars.Armor.Color1),
        ColorSequenceKeypoint.new(0.5, Config.Bars.Armor.Color2),
        ColorSequenceKeypoint.new(1, Config.Bars.Armor.Color3)
    })
    armorGradient.Rotation = 90

    cache[player].Bars.Armor = {
        Gui = armorGui,
        Outline = armorOutline,
        Frame = armorFill,
        Gradient = armorGradient
    }

    local healthGui = Instance.new("ScreenGui")
    healthGui.Name = player.Name .. "_HealthBar"
    healthGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    healthGui.Parent = game.CoreGui

    local healthOutline = Instance.new("Frame")
    healthOutline.BackgroundColor3 = Color3.new(0, 0, 0)
    healthOutline.BorderSizePixel = 0
    healthOutline.Name = "Outline"
    healthOutline.Parent = healthGui

    local healthFill = Instance.new("Frame")
    healthFill.BackgroundTransparency = 0
    healthFill.BorderSizePixel = 0
    healthFill.Name = "Fill"
    healthFill.Parent = healthOutline

    local healthGradient = Instance.new("UIGradient", healthFill)
    healthGradient.Color = ColorSequence.new({
        ColorSequenceKeypoint.new(0, Config.Bars.Health.Color1),
        ColorSequenceKeypoint.new(0.5, Config.Bars.Health.Color2),
        ColorSequenceKeypoint.new(1, Config.Bars.Health.Color3)
    })
    healthGradient.Rotation = 90

    cache[player].Bars.Health = {
        Gui = healthGui,
        Outline = healthOutline,
        Frame = healthFill,
        Gradient = healthGradient
    }
    print("Rendered ESP for " .. player.Name)
end)

utility.funcs.hide_esp = LPH_NO_VIRTUALIZE(function(player)
    if not cache[player] then return end
    if cache[player].Box and cache[player].Box.Full then
        if cache[player].Box.Full.Square then
            cache[player].Box.Full.Square.Visible = false
        end
        if cache[player].Box.Full.Outline then
            cache[player].Box.Full.Outline.Visible = false
        end
        if cache[player].Box.Full.Inline then
            cache[player].Box.Full.Inline.Visible = false
        end
        if cache[player].Box.Full.Filled then
            cache[player].Box.Full.Filled.Visible = false
        end
    end

    if cache[player].Text and cache[player].Text.BillboardGui then
        cache[player].Text.BillboardGui.Enabled = false
    end

    if cache[player].Bars then
        if cache[player].Bars.Health and cache[player].Bars.Health.Gui then
            cache[player].Bars.Health.Outline.Visible = false
            cache[player].Bars.Health.Frame.Visible = false
        end
        if cache[player].Bars.Armor and cache[player].Bars.Armor.Gui then
            cache[player].Bars.Armor.Outline.Visible = false
            cache[player].Bars.Armor.Frame.Visible = false
        end
    end
end)

utility.funcs.clear_esp = LPH_NO_VIRTUALIZE(function(player)
    if not cache[player] then return end
    if cache[player].Box and cache[player].Box.Full then
        if cache[player].Box.Full.Square then
            cache[player].Box.Full.Square:Remove()
        end
        if cache[player].Box.Full.Outline then
            cache[player].Box.Full.Outline:Remove()
        end
        if cache[player].Box.Full.Inline then
            cache[player].Box.Full.Inline:Remove()
        end
        if cache[player].Box.Full.Filled then
            cache[player].Box.Full.Filled.Parent:Destroy()
        end
    end

    if cache[player].Text and cache[player].Text.BillboardGui then
        cache[player].Text.BillboardGui:Destroy()
    end

    if cache[player].Bars then
        if cache[player].Bars.Health and cache[player].Bars.Health.Gui then
            cache[player].Bars.Health.Gui:Destroy()
        end
        if cache[player].Bars.Armor and cache[player].Bars.Armor.Gui then
            cache[player].Bars.Armor.Gui:Destroy()
        end
    end

    cache[player] = nil
end)

utility.funcs.update = LPH_NO_VIRTUALIZE(function(player)
    if not player or not player.Parent or not Players:FindFirstChild(player.Name) then
        print("Player " .. tostring(player) .. " is invalid or not found")
        return
    end
    if not cache[player] then
        utility.funcs.render(player)
        print("Initialized ESP for " .. player.Name)
    end

    local success, err = pcall(function()
        local character = player.Character
        local client_character = Players.LocalPlayer.Character
        local Camera = workspace.CurrentCamera
        local gui_inset = game:GetService("GuiService"):GetGuiInset()

        if not character or not character.Parent or not client_character or not Camera then
            print("Hiding ESP for " .. player.Name .. " due to missing components")
            utility.funcs.hide_esp(player)
            return
        end

        local rootPart = character:FindFirstChild("HumanoidRootPart")
        local head = character:FindFirstChild("Head")
        local humanoid = character:FindFirstChildWhichIsA("Humanoid")
        if not rootPart or not head or not humanoid then
            print("Hiding ESP for " .. player.Name .. " due to missing parts")
            utility.funcs.hide_esp(player)
            return
        end

        local clientRootPart = client_character:FindFirstChild("HumanoidRootPart")
        if not clientRootPart then
            print("Hiding ESP due to missing client root part")
            utility.funcs.hide_esp(player)
            return
        end
        local distance = (clientRootPart.Position - rootPart.Position).Magnitude
        if distance > Config.Distance.Max then
            print("Hiding ESP for " .. player.Name .. " due to distance: " .. distance)
            utility.funcs.hide_esp(player)
            return
        end

        local hrp2D, onScreen = Camera:WorldToViewportPoint(rootPart.Position)
        if not onScreen then
            print("Hiding ESP for " .. player.Name .. " due to off-screen")
            utility.funcs.hide_esp(player)
            return
        end

        local charSize = (Camera:WorldToViewportPoint(rootPart.Position - Vector3.new(0, 1, 0)).Y - Camera:WorldToViewportPoint(rootPart.Position + Vector3.new(0, 3, 0)).Y) / 2
        local size = Vector2.new(math.floor(charSize * 1.5), math.floor(charSize * 3.2))
        local position = Vector2.new(math.floor(hrp2D.X - charSize * 1.5 / 2), math.floor(hrp2D.Y - charSize * 3 / 2))
        local playerCache = cache[player]

        if Config.Box.Enable then
            local fullBox = playerCache.Box.Full
            local square, outline, inline, filled = fullBox.Square, fullBox.Outline, fullBox.Inline, fullBox.Filled

            if Config.Box.Type == "Full" then
                square.Visible = Config.Box.Enable
                square.Position = position
                square.Size = size
                square.Color = Config.Box.Color
                square.Thickness = 2
                square.Filled = false
                square.ZIndex = 100

                outline.Visible = Config.Box.Enable and Config.Box.Outline.Enable
                outline.Position = position - Vector2.new(1, 1)
                outline.Size = size + Vector2.new(2, 2)
                outline.Color = Config.Box.Outline.Color
                outline.Thickness = 3
                outline.Filled = false
                outline.ZIndex = 101

                inline.Visible = Config.Box.Enable
                inline.Position = position + Vector2.new(1, 1)
                inline.Size = size - Vector2.new(2, 2)
                inline.Color = Color3.new(0, 0, 0)
                inline.Thickness = 1
                inline.Filled = false
                inline.ZIndex = 99

                if Config.Box.Filled.Enable then
                    filled.Position = UDim2.new(0, position.X, 0, position.Y - gui_inset.Y)
                    filled.Size = UDim2.new(0, size.X, 0, size.Y)
                    filled.BackgroundTransparency = Config.Box.Filled.Gradient.Transparency or 0.5
                    filled.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                    filled.Visible = Config.Box.Enable
                    filled.ZIndex = -100

                    local gradient = filled:FindFirstChild("Gradient")
                    if Config.Box.Filled.Gradient.Enable and gradient then
                        gradient.Rotation = Config.Box.Filled.Gradient.Rotation.Auto and math.sin(tick() * 2) * 180 or 90
                    end
                elseif filled then
                    filled.Visible = false
                end
            else
                square.Visible = false
                outline.Visible = false
                inline.Visible = false
                if filled then filled.Visible = false end
            end
        else
            if fullBox then
                square.Visible = false
                outline.Visible = false
                inline.Visible = false
                if filled then filled.Visible = false end
            end
        end

        if Config.Text.Enable then
            local billboardGui = playerCache.Text.BillboardGui
            local nameLabel = playerCache.Text.Name
            local studsLabel = playerCache.Text.Studs
            local toolLabel = playerCache.Text.Tool

            if head and not billboardGui.Parent then
                billboardGui.Parent = head
            end

            if Config.Text.Name.Enable then
                billboardGui.Enabled = Config.Text.Enable
                nameLabel.Text = "{" .. player.Name .. "}"
                nameLabel.TextColor3 = Config.Text.Name.Color
            else
                nameLabel.Text = ""
            end

            if Config.Text.Studs.Enable then
                local meters = distance * 0.28
                studsLabel.Text = string.format("[%.0fm]", meters)
                studsLabel.TextColor3 = Config.Text.Studs.Color
            else
                studsLabel.Text = ""
            end

            if Config.Text.Tool.Enable then
                local tool = character:FindFirstChildOfClass("Tool")
                toolLabel.Text = tool and tool.Name or "none"
                toolLabel.TextColor3 = Config.Text.Tool.Color
            else
                toolLabel.Text = ""
            end
        else
            if playerCache.Text and playerCache.Text.BillboardGui then
                playerCache.Text.BillboardGui.Enabled = false
            end
        end

        if Config.Bars.Enable then
            local bar_height = size.Y
            local bar_width = 3
            local base_x = position.X
            local y = position.Y - gui_inset.Y

            if Config.Bars.Health.Enable and humanoid then
                local targetHealth = math.clamp(humanoid.Health / humanoid.MaxHealth, 0, 1)
                local lastHealth = playerCache.Bars.Health.LastHealth or targetHealth
                local lerpedHealth = lastHealth + (targetHealth - lastHealth) * 0.05
                playerCache.Bars.Health.LastHealth = lerpedHealth

                local x = base_x - (bar_width + 4)
                local outline = playerCache.Bars.Health.Outline
                local fill = playerCache.Bars.Health.Frame

                if outline and fill then
                    outline.Visible = Config.Bars.Enable
                    outline.Position = UDim2.new(0, x - 1, 0, y - 1)
                    outline.Size = UDim2.new(0, bar_width + 2, 0, bar_height + 1.1)
                    outline.BackgroundTransparency = 0.2

                    fill.Visible = Config.Bars.Enable
                    fill.Position = UDim2.new(0, 1, 0, (1 - lerpedHealth) * bar_height + 1)
                    fill.Size = UDim2.new(0, bar_width, 0, lerpedHealth * bar_height)
                end
            end

            if Config.Bars.Armor.Enable and character then
                local bodyEffects = character:FindFirstChild("BodyEffects")
                local values = bodyEffects and bodyEffects:FindFirstChild("Armor")
                local maxArmor = Config.Bars.Armor.Max or 130
                local targetArmor = values and math.clamp(values.Value / maxArmor, 0, 1) or 0

                local lastArmor = playerCache.Bars.Armor.LastArmor or targetArmor
                local lerpedArmor = lastArmor + (targetArmor - lastArmor) * 0.05
                playerCache.Bars.Armor.LastArmor = lerpedArmor

                local x = base_x - (bar_width * 2 + 6 + 2)
                local outline = playerCache.Bars.Armor.Outline
                local fill = playerCache.Bars.Armor.Frame

                if outline then
                    outline.Visible = Config.Bars.Enable
                    outline.Position = UDim2.new(0, x - 1, 0, y - 1)
                    outline.Size = UDim2.new(0, bar_width + 2, 0, bar_height + 1.1)
                    outline.BackgroundTransparency = 0.2
                end

                if fill then
                    fill.Visible = Config.Bars.Enable
                    fill.Position = UDim2.new(0, 1, 0, (1 - lerpedArmor) * bar_height + 1)
                    fill.Size = UDim2.new(0, bar_width, 0, lerpedArmor * bar_height)
                end
            end
        else
            if playerCache.Bars then
                if playerCache.Bars.Health and playerCache.Bars.Health.Gui then
                    playerCache.Bars.Health.Outline.Visible = false
                    playerCache.Bars.Health.Frame.Visible = false
                end
                if playerCache.Bars.Armor and playerCache.Bars.Armor.Gui then
                    playerCache.Bars.Armor.Outline.Visible = false
                    playerCache.Bars.Armor.Frame.Visible = false
                end
            end
        end
    end)

    if not success then
        warn("Error updating ESP for " .. tostring(player) .. ": " .. err)
        utility.funcs.hide_esp(player)
    end
end)

-- Initialize aura and forcefield for local player
spawnAura()
if player.Character then
    onCharacterAdded(player.Character)
end
player.CharacterAdded:Connect(onCharacterAdded)

-- Separate forcefield update loop
connections.forcefield = RunService.RenderStepped:Connect(function(deltaTime)
    if not player or not player.Parent then return end
    lastUpdateTime = lastUpdateTime + deltaTime
    if lastUpdateTime >= updateInterval then
        lastUpdateTime = 0
        if Config.Forcefield.Enabled then
            local character = player.Character
            if character then
                customizeCharacter(character, Config.Forcefield.Color)
            end
        end
    end
end)

-- Initialize ESP for all players on start
for _, p in pairs(Players:GetPlayers()) do
    if p ~= player then
        utility.funcs.render(p)
    end
end

-- Handle new players joining
Players.PlayerAdded:Connect(function(newPlayer)
    if newPlayer ~= player then
        utility.funcs.render(newPlayer)
    end
end)

-- Main render loop
connections.main = connections.main or {}
connections.main.RenderStepped = game:GetService("RunService").PostSimulation:Connect(function(deltaTime)
    if not player or not player.Parent then return end
    for p, _ in pairs(cache) do
        if p and p.Parent and Players:FindFirstChild(p.Name) then
            utility.funcs.update(p)
        else
            utility.funcs.clear_esp(p)
        end
    end

    -- Update aura (always active if enabled)
    if player and player.Character and player.Character:FindFirstChild("LowerTorso") then
        if not Attachment.Parent then
            Attachment.Parent = player.Character.LowerTorso
            initializeAura(Config.Aura.Type)
        end
        for _, particle in pairs(currentParticles) do
            if particle then
                particle.Enabled = Config.Aura.Enable
            end
        end
    else
        for _, particle in pairs(currentParticles) do
            if particle then
                particle.Enabled = false
            end
        end
    end
end)
