# Quick Set-up for HLSL

!!! info

    The HLSL setup is identical to what we did in ShaderGraph setup;

    1. Create Texture2D

    2. Change reference name to have prefix '____BakeGrad____'

    3. Make the shader use the custom editor "EvernullTools.GradientGUI"

    4. Use it as any other Gradient Texture

```hlsl
Shader "Unlit/gradientInputTutorial"
{
    Properties
    {
        _MainTex ("Texture", 2D) = "white" {}
        _BakeGrad_GradientTexture ("Texture", 2D) = "white" {} 
        // -------> This texture has the prefix '_BakeGrad_' <-------
    }
    SubShader
    {
        Tags { // ... some shader tags ... // }
        LOD 100

        Pass
        {
            CGPROGRAM
            {
            // ... some shader code ... //
                return color;
            }
            ENDCG
        }
    }
    CustomEditor "EvernullTools.GradientGUI" 
    // -------> Shader is using CustomEditor: "EvernullTools.GradientGUI" <-------
}

```