# Exemplos ImageNet

Esta pasta contém 2 exemplos que demonstram como usar redes convertidas para
classificação de imagens. Também inclusas estão amostras de modelos convertidos e scripts de suporte.

## 1. Classificação de Imagens

`classify.py` usa a rede GoogleNet treinada na ImageNet, convertida para TensorFlow, para classificar imagens.

A arquitetura usada é definida em `models/googlenet.py` (que é auto-generada). Vocẽ irá precisar baixar
e converter os pesos do Caffe para rodar o exemplo. O link de download para os pesos
correspondentes podem ser encontrados na pasta `models/bvlc_googlenet/` do Caffe.

Vocẽ pode rodar este exemplo mais ou menos assim:

    $ ./classify.py /path/to/googlenet.npy ~/pics/kitty.png ~/pics/woof.jpg

Vocẽ deverá ter uma saída similar a isso:

    Image                Classified As                  Confidence
    ----------------------------------------------------------------------
    kitty.png            Persian cat                    99.75 %
    woof.jpg             Bernese mountain dog           82.02 %


## 2. Validação ImageNet

`validate.py` computará um modelo convertido perante a base de validação ImageNet (ILSVRC12). Para rodar este
script, você irá precisar de uma cópia da base de validação da ImageNet. Vocẽ pode roda-la conforme abaixo:

    $ ./validate.py alexnet.npy val.txt imagenet-val/ --model AlexNet

Os resultados de validação especificados no readme principal gerado onde foi rodado este script.

## Scripts de ajuda ou de suporte

Em adição aos exemplos acima, esta pasta inclui alguns poucos arquivos adicionais:

- `dataset.py` : helper script para carregar, pre-processar, e interagir entre as imagens
- `models/` : contem modelos convertidos (auto-gerados)
- `models/helper.py` : descrevem como os dados devem ser preprocessados para cada modelo

