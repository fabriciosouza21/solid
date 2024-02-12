
*Uma classe tem um bom design, quando a classe só tem um  interessado*

*uma classes com muitos interessados, por diversas mudanças*

*uma classes tem muitas responsabilidades quando tem vários interessados*

muitas responsabilidades significa muitas razões pra ser modificada

### Single responsbility principle (SRP)
Uma classe deve ter um, e apenas um, motivo para ser modificada

### SRP E CLASSES COESAS

#### Adesão 
Quando algo externo, gruda coisas relacionadas

#### Coesão
quando a união é natural

classes coesa têm uma característica semelhante: os conceitos que essas classes representam estariam relacionado e separá-los  seria pouc natural

### como identificar classes pouco coesas
- Possuem muitos métodos diferentes
- sao modificadas com frequência
- não para nunca de crescer

#### imports
Os imports podem ser um bom indicador de muitas responsabilidades, classes que utilizam muitos pacotes diferentes tem grande chancer de precisaram ser modificadas por vários motivos

