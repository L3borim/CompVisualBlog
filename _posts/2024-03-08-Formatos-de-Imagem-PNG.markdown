---
layout: post
title:  "Formatos de Imagem: PNG"
date:   2024-08-03 10:50:00 -0300
categories: Semana03 ComputacaoVisual
---

O formato PNG consiste em uma assinatura de identificação de 8 bytes seguida de 8 ou mais blocos de dados (chunks). esses chunks carregam sua própria identificação em relação ao seu formato interno e são lidos sequencialmente do início ao fim do arquivo ou fluxo de dados.  

Os primeiros oito bytes de um arquivo PNG sempre contêm os seguintes valores:

>**(decimal)**   `137  80  78  71  13  10  26  10`
>**(hexadecimal)**    `89  50  4e  47  0d  0a  1a  0a`
>**(ASCII C notation)**    `\211   P   N   G  \r  \n \032 \n`

Essa assinatura indica que o restante do arquivo contém uma única imagem PNG, consistindo em uma série de pedaços (chunks) que começam com um chunk IHDR e terminam com um chunk IEND.
PNG define quatro chunks padrão, chamados de chunks críticos, que devem ser suportados por todo leitor e escritor de arquivos PNG. Esses pedaços são os seguintes:

* **IHDR** – _The header chunk_ (chunk de Cabeçalho da Imagem) que contém a informação básica da imagem, pode haver apenas um e como dito antes deve ser o primeiro.  

* **PLTE** - _The pallete chunk_ (chunk de paleta) que armazena os dados do mapa de cores associados aos dados da imagem. Este chunk está presente apenas se os dados da imagem usarem uma paleta de cores e deve aparecer antes do pedaço de dados da imagem.  

* **IDAT** - _The image data chunk_ (chunk de dados da imagem) este chunk armazena os dados reais da imagem e vários chunks de dados da imagem podem ocorrer em um fluxo de dados e devem ser armazenados em ordem contígua.
* **IEND** - _The image trailer chunk_ (chunk de fim de imagem) deve ser o último chunk e marca o final do arquivo PNG ou do fluxo de dados.  

Desses chunks citados, IHDR, IDAT e IEND devem aparecer em todos os fluxos de dados de um PNG.

<div align="center">

![formato-imagem-png](../_images/post_semana03/png-file-format.png)

</div>

Cada chunk consiste em quatro partes:  

1. **Comprimento**: Um inteiro não assinado de 4 bytes que indica o número de bytes no campo de dados do chunk. O comprimento conta apenas o campo de dados, não ele próprio, o código do tipo de chunk ou o CRC. Zero é um comprimento válido. Embora codificadores e decodificadores devam tratar o comprimento como não assinado, seu valor não deve exceder 231 bytes.  

2. **Tipo de Chunk**: Um código de tipo de chunk de 4 bytes. Para conveniência na descrição e na análise de arquivos PNG, os códigos de tipo são restritos a consistir em letras ASCII maiúsculas e minúsculas. No entanto, codificadores e decodificadores devem tratar os códigos como valores binários fixos, não como cadeias de caracteres. Por exemplo, não seria correto representar o código do tipo IDAT pelos equivalentes em EBCDIC dessas letras.  

3. **Dados do Chunk**: Os bytes de dados apropriados ao tipo de chunk, se houver. Este campo pode ter comprimento zero.  

4. **CRC**: Um _Cyclic Redundancy Check_ ou CRC de 4 bytes calculado nos bytes anteriores do chunk, incluindo o código do tipo de chunk e os campos de dados do chunk, mas não incluindo o campo de comprimento. O CRC está sempre presente, mesmo para chunks que não contêm dados.  

Ao utilizar a ferramenta [Nayuki](https://www.nayuki.io/page/png-file-chunk-inspector), que funciona como um inspetor de chunks de imagens em PNG, para dissecar o png (figura 1) podemos verificar a lista de chunks e possíveis erros que violam o formato (figura 2).

![Yuta-example-img](../_images/post_semana03/Yuta_image.png)
_Figura 1_

![Nayuki-result-table](../_images/post_semana03/Nayuki-result-table.png)
_Figura 2_

`Referências Bibliográficas:`  

W3C. Portable Network Graphics (PNG) Specification (Second Edition). 10 Nov. 2003. Disponível em: [https://www.w3.org/TR/2003/REC-PNG-20031110/#4Concepts.PNGImage](https://www.w3.org/TR/2003/REC-PNG-20031110/#4Concepts.PNGImage). Acesso em 08/03/2024

Graphics File Formats. 2nd ed. Sebastopol: O'Reilly, 1996. Disponível em: [https://vintageapple.org/macbooks/pdf/Graphics_File_Formats_Second_Edition_1996.pdf](https://vintageapple.org/macbooks/pdf/Graphics_File_Formats_Second_Edition_1996.pdf). Acesso em: 08/03/2024

PNG Development Group. Portable Network Graphics (PNG) Specification (Second Edition). Disponível em: [http://www.libpng.org/pub/png/spec/1.2/PNG-Structure.html.](http://www.libpng.org/pub/png/spec/1.2/PNG-Structure.html.) Acesso em: 08/03/2024.

Nayuki. PNG File Chunk Inspector. Disponível em: [https://www.nayuki.io/page/png-file-chunk-inspector](https://www.nayuki.io/page/png-file-chunk-inspector). Acesso em: 08/03/2024
