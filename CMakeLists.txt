cmake_minimum_required(VERSION 3.8)
project(directxtk)

if(WIN32 AND NOT WINDOWS_STORE)
    set(WINDOWS_DESKTOP 1)
else()
    set(WINDOWS_DESKTOP 0)
endif()

include_directories(Inc Src ${CMAKE_CURRENT_BINARY_DIR}/include)

set(AUDIO_FILES
    Inc/Audio.h
    Audio/AudioEngine.cpp
    Audio/DynamicSoundEffectInstance.cpp
    Audio/SoundCommon.cpp
    Audio/SoundCommon.h
    Audio/SoundEffect.cpp
    Audio/SoundEffectInstance.cpp
    Audio/WaveBank.cpp
    Audio/WaveBankReader.cpp
    Audio/WaveBankReader.h
    Audio/WAVFileReader.cpp
    Audio/WAVFileReader.h
)

set(COMMON_INCLUDES 
    Inc/CommonStates.h
    Inc/DDSTextureLoader.h
    Inc/DirectXHelpers.h
    Inc/Effects.h
    Inc/GeometricPrimitive.h
    Inc/GraphicsMemory.h
    Inc/Model.h
    Inc/PostProcess.h
    Inc/PrimitiveBatch.h
    Inc/ScreenGrab.h
    Inc/SpriteBatch.h
    Inc/SpriteFont.h
    Inc/VertexTypes.h
    Inc/WICTextureLoader.h
#    Inc/XboxDDSTextureLoader.h
)

set(COMMON_SHARED_INCLUDES
    Inc/GamePad.h
    Inc/Keyboard.h
    Inc/Mouse.h
    Inc/SimpleMath.h
    Inc/SimpleMath.inl
)

set(COMMON_SHARED_SOURCES 
    Src/AlignedNew.h
    Src/Bezier.h
    Src/BinaryReader.h
    Src/BinaryReader.cpp
    Src/dds.h
    Src/DemandCreate.h
    Src/GamePad.cpp
    Src/Geometry.cpp
    Src/Geometry.h
    Src/Keyboard.cpp
    Src/LoaderHelpers.h
    Src/Mouse.cpp
    Src/PlatformHelpers.h
    Src/SDKMesh.h
    Src/SharedResourcePool.h
    Src/vbo.h
    Src/SimpleMath.cpp
    Src/TeapotData.inc
)

set(COMMON_SOURCES 
    Src/AlphaTestEffect.cpp
    Src/BasicEffect.cpp
    Src/BasicPostProcess.cpp
    Src/CommonStates.cpp
    Src/DDSTextureLoader.cpp
    Src/DebugEffect.cpp
    Src/DGSLEffect.cpp
    Src/DGSLEffectFactory.cpp
    Src/DualPostProcess.cpp
    Src/DualTextureEffect.cpp
    Src/EffectCommon.cpp
    Src/EffectFactory.cpp
    Src/EnvironmentMapEffect.cpp
    Src/GeometricPrimitive.cpp
    Src/GraphicsMemory.cpp
    Src/Model.cpp
    Src/ModelLoadCMO.cpp
    Src/ModelLoadSDKMESH.cpp
    Src/ModelLoadVBO.cpp
    Src/NormalMapEffect.cpp
    Src/PBREffect.cpp
    Src/pch.cpp
    Src/PrimitiveBatch.cpp
    Src/ScreenGrab.cpp
    Src/SkinnedEffect.cpp
    Src/SpriteBatch.cpp
    Src/SpriteFont.cpp
    Src/ToneMapPostProcess.cpp
    Src/VertexTypes.cpp
    Src/WICTextureLoader.cpp
 #   Src/XboxDDSTextureLoader.cpp

    Src/ConstantBuffer.h
    Src/EffectCommon.h
    Src/pch.h
)

file(GLOB SHARED_SHADERS
    "Src/Shaders/Common.fxh" 
    "Src/Shaders/Lighting.fxh" 
    "Src/Shaders/PBRCommon.fxh" 
    "Src/Shaders/Structures.fxh" 
    "Src/Shaders/Utilities.fxh"
    "Src/Shaders/PixelPacking_Velocity.hlsli"
)

file(GLOB COMPILED_SHADERS
    "Src/Shaders/Compiled/*.inc" 
)

file(GLOB SYMBOLS_SHADERS
    "Src/Shaders/Compiled/*.pdb" 
)

file(GLOB COMMON_SHADERS
    "Src/Shaders/*.fx"
    "Src/Shaders/*.cmd" 
)

source_group(Audio FILES ${AUDIO_FILES})

source_group(Inc FILES ${COMMON_INCLUDES})
source_group(Inc\\Shared FILES ${COMMON_SHARED_INCLUDES})

source_group(Src FILES ${COMMON_SOURCES})
source_group(Src\\Shared FILES ${COMMON_SHARED_SOURCES})

source_group(Src\\Shaders FILES ${COMMON_SHADERS})
source_group(Src\\Shaders\\Shared FILES ${SHARED_SHADERS})
source_group(Src\\Shaders\\Symbols FILES ${SYMBOLS_SHADERS})
source_group(Src\\Shaders\\Compiled FILES ${COMPILED_SHADERS})

add_library(directxtk STATIC 
    ${COMMON_SOURCES} 
    ${COMMON_INCLUDES} 
    ${SHARED_SHADERS}
    ${AUDIO_FILES} 
    ${COMMON_SHARED_INCLUDES}
    ${COMMON_SHARED_SOURCES}
    ${COMMON_SHADERS}
    ${SHARED_SHADERS}
    ${SYMBOLS_SHADERS}
    ${COMPILED_SHADERS}
)

target_include_directories(directxtk PUBLIC "${CMAKE_CURRENT_LIST_DIR}/Inc")

set_target_properties(directxtk PROPERTIES COMPILE_FLAGS "/Yupch.h")
set_source_files_properties(Src/pch.cpp PROPERTIES COMPILE_FLAGS "/Ycpch.h")
