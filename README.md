# Detecção de Lesões de Retinopatia Diabética em Imagens de Fundoscopia Utilizando UNet++
[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ermeson-alves/UNetpp-RD/blob/main/UNet%2B%2B_Single_Class_ic_2022_2023.ipynb)

![](https://github.com/ermeson-alves/UNetpp-RD/blob/main/imgs/ex-predict.png)

A Retinopatia Diabética é uma complicação microvascular do *Diabets Melitus* que causa anormalidades na retina, em casos mais graves podendo levar a cegueira. O avanço das técnicas de Deep Learning no campode Visão computacional tem se tornado fundamental para o dignóstico precoce dessa doença, ainda mais pela possibilidade de evitar um quadro mais crítico da mesma. O artigo referente ao código desse repositório se propõe a analisar o desempenho de diferentes configurações da rede de Segmentação Semântica UNet++, testando diferentes redes codificadoras no *backbone* dessa arquitetura.


O trabalho foi realizado totalmente com auxílio da plataforma **Google Colab** a fim de se obter um hardware com maior capacidade de processamento dos dados.

## Índice

- [Instruções de uso](#instruções-de-uso)
- [Tópicos do Colab](#tópicos-do-colab)
- [Contato](#contato)



## Instruções de uso

1. O conjunto de dados IDRID pode ser baixado [neste link](https://ieee-dataport.org/open-access/indian-diabetic-retinopathy-image-dataset-idrid), o diretório utilizado será o `A. Segmentation.zip`. Infelizmente, a versão do conjunto de dados DIARETD que foi utilizada para o trabalho (DIARETDB1v1.1) não se encontra disponível, mas outras versões podem ser adaptadas para uso, como o [DIARETDB0v1.1](https://www.kaggle.com/datasets/nguyenhung1903/diaretdb1-standard-diabetic-retinopathy-database). <br><br> Após o download dos dados deve-se criar uma pasta de trabalho no Google Drive e enviar os dados para ela. Com o software Google Drive Desktop, para Windows e Mac, o download pode ser feito diretamente para o Google Drive.

2. Realiza-se a montagem dos arquivos do Google Drive no colab bem como muda-se o **work directory** para a pasta criada anteriormente. Exemplo:

    ```python
    from pathlib import Path
    import os
    # Path para o diretório de trabalho:
    os.chdir("/content/drive/MyDrive/DRIVE COMPARTILHADO/PDI -2/UNet-SegRD")
    Path.cwd()
    ```

3. Necessário usar `pythorch lighting 1.9.2` e instalar a biblioteca `segmentations models pytorch`:

    ```python
    %pip install -q segmentation_models_pytorch
    %pip install -q pytorch_lightning==1.9.2
    ```
4. No Treino Da UNet++, ao definir os objetos Trainers é necessário passar um path para armazenar os checkpoints do modelo. Uma outra pasta do Google Drive a ser escolhida:

    ```python
      checkpoint_callback = ModelCheckpoint(
        dirpath = f'./checkpoints/treino-single-class/', # Diretório de checkpoints
        filename=f'Unet++_{seg_encoder}_{LESIONS_DIARET[type_dataset]}',
        monitor='valid_dataset_iou',
        mode="max",
      )
    ```


5. Os Tópicos do Colab separam as principais partes do treino ao teste dos modelos.

## Tópicos do Colab
<details>
<summary>Visualização</summary>
<br>

> Código auxiliar para exibir um lote de imagens e as respectivas mascaras.
</details>

<!-- -------------------- -->
<details>
<summary>Librarys</summary>
<br>

> Configurações necessárias de bibliotecas python para o treinamento, conforme instrução de uso 3.
</details>
<!-- -------------------- -->
<details>
<summary>Load Dataset</summary>

+   <details>
    <summary>Definição dos Datasets e Dataloaders
    </summary>
    <br>

    > Com base no dataset escolhido, IDRID ou DIARETDB, e dos respectivos paths, gera os objetos pytorch para os dataset e dataloaders. Ver: [Datasets & DataLoaders](https://pytorch.org/tutorials/beginner/basics/data_tutorial.html).
    </details>

+   <details>
    <summary>Plot</summary>
    <br>

    > Plota uma amostra dos dados por fins de análise do resultado do código anterior e correção de erros, se for o caso.
    </details>

</details>
<!-- -------------------- -->
<details>
<summary>Treino U-Net++</summary>

+   <details>
    <summary>Definição do Lighting Module
    </summary>
    <br>

    > Ver [LightningModule](https://lightning.ai/docs/pytorch/stable/common/lightning_module.html). Classe pytorch lighting que ajuda a modularizar os trabalhos de treino e teste com pytorch.
    </details>

+   <details>
    <summary>Definição Trainers</summary>
    <br>

    > Onde o treino é executado de fato. Ver [Trainer](https://lightning.ai/docs/pytorch/stable/common/trainer.html)
    </details>
</details>
<!-- -------------------- -->
<details>
<summary>Teste e Plot de Predições</summary>

> Recria datasets e dataloaders de teste de Microaneurismas, Hemorragias, Exsudatos duros, Exsudatos e Disco Óptico e aplica as méticas de estudo para análise.

+   <details>
    <summary>Plot
    </summary>
    <br>

    > Para cada subdataset do conjunto em questão, é plotado a imagem original, a sua mascara e as predições com as diferentes configurações sugeridas.
    </details>
</details>
<!-- -------------------- -->

## Contato
Email: ermeson.alves@lesc.ufc.br
