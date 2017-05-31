# Caffe para TensorFlow

Converte modelos [Caffe](https://github.com/BVLC/caffe/) para [TensorFlow](https://github.com/tensorflow/tensorflow).

## Uso

Execute `convert.py` para converter um modelo Caffe existente para TensorFlow.

Tenha certeza que você está usando o último formato Caffe (veja a seção de notas para mais informações).

A saida consiste em dois arquivos:

1. Um arquivo de dados (em formato NumPy's nativo) contendo os parâmetros aprendidos do modelo.
2. Uma classe python que constroi o grafo do modelo.

### Examplos

Veja a pasta [examples](examples/) para mais detalhes.

## Verificação

Os modelos convertidos abaixo foram verificados no conjunto de validacao ILSVRC2012 usando
[validate.py](examples/imagenet/validate.py).

| Modelo                                                | Top 5 Accuracy |
|:------------------------------------------------------|---------------:|
| [ResNet 152](http://arxiv.org/abs/1512.03385)         |         92.92% |
| [ResNet 101](http://arxiv.org/abs/1512.03385)         |         92.63% |
| [ResNet 50](http://arxiv.org/abs/1512.03385)          |         92.02% |
| [VGG 16](http://arxiv.org/abs/1409.1556)              |         89.88% |
| [GoogLeNet](http://arxiv.org/abs/1409.4842)           |         89.06% |
| [Network in Network](http://arxiv.org/abs/1312.4400)  |         81.21% |
| [CaffeNet](http://arxiv.org/abs/1408.5093)            |         79.93% |
| [AlexNet](http://goo.gl/3BilWd)                       |         79.84% |

## Notas

- Apenas o novo modelo Caffe é suportado. Se vocẽ tem um modelo antigo, use as ferramentas `upgrade_net_proto_text` e `upgrade_net_proto_binary` que vem junto com o Caffe para atualiza-los primeiro. Primeiramente tenha certeza que está usando uma versão bem recente do Caffe.

- Aparentemente Caffe e TensorFlow não podem ser invocados concorrentemente (conflitos CUDA - mesmo que você rode `set_mode_cpu`). Isto acaba resultando em um processo dois estágios: primeiro extraia os parâmetros com `convert.py`, então importe-os no TensorFlow.

- Caffe não é necessáriamente requerido. Se PyCaffe estiver disponível no seu `PYTHONPATH`, e a variável `USE_PYCAFFE` estiver setada, ele será usado. De outra forma, um fallback será usado. De qualquer jeito, o fallback acaba usando uma implementação puramente em python de `protobuf`, que é impressionantemente lenta (~1.5 minutos para ler os parâmetros VGG16). Um backend experimental de protobuf em c++ particularmente não ajuda muito aqui, uma vez que ele roda no limite do tamanho do arquivo (Caffe resolve esse problema sobrepondo o este limite em C++). Uma solução limpa aqui seria implementar o carregador como um módulo C++ .

- Só um subconjunto das camadas de Caffe e parâmetros de acompanhamento são atualmente suportados.

- Nem todos os modelos de Caffe podem ser convertidos para TensorFlow. No caso, Caffe suporta preenchimento arbitrário onde o respectivo suporte do TensorFlow atualmente é restrito a `SAME` e `VALID`.

- Os valores de borda são tratados diferentemente por Caffe e TensorFlow. De toda forma, isso não parece afetar muito as coisas.

- Redimensionamento de imagem pode afetar o ILSVRC2012 top 5 accuracy listado acima sensivelmente. VGG16 espera um redimensionamento isotropico (anisotropico reduz a precisão a 88.45%) onde a implementação BVLC da GoogLeNet espera redimensionamento anisotropico (isotropico reduz a precisão a 87.7%).

- O suporte a classe `kaffe.tensorflow.Network` não tem dependẽncias internas. Ela pode ser seguramente retirada e distribuida sem o resto da biblioteca.

- O modelo ResNet usa convoluções 1x1 com um passo de 2. Isso é suportado atualmente apenas no master branch do TensorFlow (a última compilação estável no momento em que este artigo estava sendo escrito era v0.8.0, que não suportava tal funcionalidade).
