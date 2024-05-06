---
layout: post
title:  "APIs Gráficas"
date:   2024-05-05 10:48:48 -0300
categories: Semana08 ComputacaoVisual
---

O termo API significa Application Programming Interface, em português, Interface de Programação de Aplicativos. Ele é um conjunto de padrões de programação que permite a construção de aplicativos, sendo uma forma de dois ou mais programas do computador se comunicarem entre si através de uma série de normas e protocolos. E uma API gráfica é um conjunto de regras que permite ao software enviar informações de polígonos 3D para GPU, sendo dedicada à área de imagem e processamento gráfico 3D.


A API gráfica escolhida para a pesquisa foi o DirectX, criada pela Microsoft presente em grandes partes dos jogos eletrônicos. Essa API permite aos softwares dar instruções diretas para os componentes de hardware de áudio e vídeo, melhorando o desempenho das aplicações na execução dos recursos multimídia, sendo muito presente nos jogos já que eles demandam mais aceleração gráfica dos PCs/notebooks.

Linguagens que são suportadas e usadas na API são, CG, se destacando por ter estruturas semelhantes a do C para comandos, podendo ser usado tanto para programas feitos por DirectX, quanto por OpenGL; HLSL, parecido com a linguagem CG, resultado da parceria de desenvolvimento de uma linguagem de shading entre Microsoft e Nvidia.

Toda a documentação oficial da API DirectX é fornecida e atualizada por meio do site da sua empresa desenvolvedora, a Microsoft. O pipeline programável do Direct3D foi projetado com intuito de gerar gráficos para aplicativos de jogos em tempo real. Com base no artigo documentado pela Microsoft, há duas interfaces que definem o pipeline de gráficos, sendo a ID3D11Device responsável pela representação virtual da GPU e seus recursos, e a ID3D11DeviceContext capaz de processar graficamente esses recursos em cada estágio para o pipeline.

Antes mesmo do console Xbox ter seu nome definitivo, tal tecnologia foi decisiva para nomeá-lo durante seu desenvolvimento, possuindo como seu primeiro nome original DirectXbox. Dessa forma, podemos ter noção de como essa API é fundamental para o kit de desenvolvimento de software (SDK) do console, sendo base para criação de jogos e aplicativos para a plataforma.

Abaixo temos um exemplo de código de shader (de pixel) escrito em HLSL e suportado pela DirectX que usa as coordenadas de textura para determinar e devolver a cor de cada pixel.

```
struct VS_OUTPUT{
    float4 position : SV_POSITION;
    float2 texCoord : TEXCOORD0;
};

Texture2D textureMap : register(t0);
SamplerState sampler : register(s0);

float4 PS_SimpleShader(VS_OUTPUT input) : SV_TARGET {
    float4 color = textureMap.Sample(sampler, input.texCoord);
    return color;
}

```

O código abaixo, fornecido pelo site da Microsoft, provém de um artigo que busca uma introdução rápida acerca do uso do Direct2D. Sua função é relacionada a criação de um retângulo partindo de algumas etapas: cria recursos de desenho, descreve a forma que será desenhada, desenha ela, e em seguida libera os recursos de desenho utilizados.

```

DX::ThrowIfFailed(
        D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            __uuidof(ID2D1Factory1),
            &options,
            &m_d2dFactory
            )
        );

// Obtain the underlying DXGI device of the Direct3D11.1 device.
    DX::ThrowIfFailed(
        m_d3dDevice.As(&dxgiDevice)
        );

    // Obtain the Direct2D device for 2-D rendering.
    DX::ThrowIfFailed(
        m_d2dFactory->CreateDevice(dxgiDevice.Get(), &m_d2dDevice)
        );

    // And get its corresponding device context object.
    DX::ThrowIfFailed(
        m_d2dDevice->CreateDeviceContext(
            D2D1_DEVICE_CONTEXT_OPTIONS_NONE,
            &m_d2dContext
            )
        );

// Now we set up the Direct2D render target bitmap linked to the swapchain. 
    // Whenever we render to this bitmap, it will be directly rendered to the 
    // swapchain associated with the window.
    D2D1_BITMAP_PROPERTIES1 bitmapProperties = 
        BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_PREMULTIPLIED),
            m_dpi,
            m_dpi
            );

    // Direct2D needs the dxgi version of the backbuffer surface pointer.
    ComPtr<IDXGISurface> dxgiBackBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiBackBuffer))
        );

    // Get a D2D surface from the DXGI back buffer to use as the D2D render target.
    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiBackBuffer.Get(),
            &bitmapProperties,
            &m_d2dTargetBitmap
            )
        );

    // So now we can set the Direct2D render target.
    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());

ComPtr<ID2D1SolidColorBrush> pBlackBrush;
DX::ThrowIfFailed(
   m_d2dContext->CreateSolidColorBrush(
        D2D1::ColorF(D2D1::ColorF::Black),
        &pBlackBrush
        )
);

m_d2dContext->BeginDraw();

m_d2dContext->DrawRectangle(
    D2D1::RectF(
        rc.left + 100.0f,
        rc.top + 100.0f,
        rc.right - 100.0f,
        rc.bottom - 100.0f),
        pBlackBrush);

DX::ThrowIfFailed(
    m_d2dContext->EndDraw()
);

DX::ThrowIfFailed(
    m_swapChain->Present1(1, 0, &parameters);
);

```

`Links de Referência:`
* [https://learn.microsoft.com/pt-br/windows/win32/direct3dgetstarted/understand-the-directx-11-2-graphics-pipeline](https://learn.microsoft.com/pt-br/windows/win32/direct3dgetstarted/understand-the-directx-11-2-graphics-pipeline)
* [https://www.tecmundo.com.br/software/900-o-que-e-directx-.htm](https://www.tecmundo.com.br/software/900-o-que-e-directx-.htm)
* [https://learn.microsoft.com/pt-br/windows/win32/direct2d/direct2d-quickstart-with-device-context](https://learn.microsoft.com/pt-br/windows/win32/direct2d/direct2d-quickstart-with-device-context)
* [http://desenvolvimentodejogos.wikidot.com/shaders#:~:text=Linguagem%20de%20Shading%20que%20surgiu,qual%20acompanha%20o%20DirectX%2011.](http://desenvolvimentodejogos.wikidot.com/shaders#:~:text=Linguagem%20de%20Shading%20que%20surgiu,qual%20acompanha%20o%20DirectX%2011.)
* [https://blog.2amgaming.com/2019/09/o-que-e-api-grafica
](https://blog.2amgaming.com/2019/09/o-que-e-api-grafica/
)
* [https://www.tecmundo.com.br/software/900-o-que-e-directx-.htm
](https://www.tecmundo.com.br/software/900-o-que-e-directx-.htm
)
* [https://aws.amazon.com/pt/what-is/api/
](https://aws.amazon.com/pt/what-is/api/
)
