# FAQ

??? question "Why are gradients stored as textures?"

    Shaders work in your GPU. While being really fast for certain situations, GPU's only understand a limited set of data types such as numbers, vectors, textures etc.

    Think of the GPU as a highly specialized technician. It is extremely good at a few specific jobs, but it doesn't understand concepts outside of its own toolbox.

    Shaders just know about colors. A Gradient isn't just a list of colors. They include colors, keys that remember those colors, ways to blend and lerp those colors, percentage/positions of keys.
    It is not a simple Vector4 just like a Color is. A shader has no concept of this structure.

    Textures, on the other hand, are one of the GPU's native data types. GPU's can render, sample, manipulate textures really well.

    Since we are making games, we really want to limit the real-time calculations and we want to give the GPU something it knows how to handle. 

    At runtime, the shader simply samples a tiny texture, making it just as efficient as using any other gradient texture you imported manually.

    GPU sees texture.

    GPU is happy.

    User sees gradient.

    User is happy.

    Everyone deals with the data they intuitively understand.

??? question "Can I use this in my HLSL shader?"

    Absolutely, the implementation has no ShaderGraph specific parts. [Quick Set-up for HLSL](quick_setup_hlsl.md) shows you how you can set it up.

??? question "Can I use more than one gradient in the same shader?"

    Yes. Simply create multiple Texture2D properties and give each one a  prefix '____BakeGrad____' in the reference name. Each property will receive its own editable gradient and baked texture.

??? question "Does this work in URP/HDRP/Built-In?"

    Yes. ShaderGradients is renderer agnostic. It simply generates textures and custom material inspectors, so it works anywhere Texture2D properties work.

??? question "Does this increase build size?"

    Only by the size of the baked textures. By default each gradient is just a 256x1 texture, so the footprint is extremely small. ~2KB per texture. (Can be further reduced by reducing texture resolution)

??? question "What happens if I duplicate a material?"

    ShaderGradients automatically detects duplicated materials and creates independent gradient textures for the new material, so editing one material never changes another.

??? question "Can I bake textures at runtime?"

    Can you? Absolutely.

    You can definitely bake these at runtime. The baked textures are very small and you can definitely bake new textures when some other gradient is passed in. 
    Using the same functions that EvernullTools.GradientGUI uses internally.

    Should you? Probably not.
    It's very hard to imagine any situation where baking at runtime is better than simply just baking in editor and storing those 1KB textures. These textures are so small in memory that 
    the cost of baking one is likely always gonna be much more expensive than storing hundreds of these. I honestly wouldn't bake them on runtime, but I can't stop you from doing it :D
    
    If you wanted to change between a few gradients at runtime, I'd still just store them and lerp between them in the shader at runtime.

??? question "Can I rename the property later?"

    Yes. The old gradient will become orphaned and GradientData will offer a cleanup button the next time it's inspected. If you don't want to lose your gradient, either copy it or simply save it as a preset.

??? question "Does this create runtime allocations?"

    No. The baking happens in the Unity Editor. At runtime you're simply sampling a texture like any other shader.