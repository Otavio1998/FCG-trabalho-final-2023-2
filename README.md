# FCG-trabalho-final-2023-2
Trabalho final de Fundamentos de Computação Gráfica

Descrição:
  O trabalho consiste em um jogo onde um gato(chamado Python) precisa subir na cama e derrubar o objeto que esta em cima dela. A linguagem utilizada no trabalho é C++, fazendo o uso de OpenGL como API grafica.
  Para o trabalho foi utilizado a IDE CodeBlocks no desenvolvimento, junto com todas as bibliotecas fornecidas pelo professor.

Modelos:
  Foram carregados os modelos do gato e quarto, encontrado na internet com seus arquivos de textura, e o modelo da esfera, ja fornecido pelo professor como parte dos laboratorios. O mapeamento é feito no arquivo shader_vertex.glsl.
  O tipo de mapeamento usado para os modelos do gato e da sala é projeção planar, enquanto que o mapeamento da esfera é UV unwrapping esferico.
  Os modelos em si são desenhados no main.cpp, onde também são definidas as suas posições(O gato depende de cat_position, o quarto pode ter valores constantes e a esfera depende de sphere_position).

iluminação:
  Para a ilumunação foram utilizadas os modelos de iluminaçaõ de Lambert(gato e quarto) e Blinn-Phon(para a esfera), ambos também sendo feitos no arquivo shader_vertex.glsl

camera:
  a camera implementada é lookat, fixada nas costas do gato e também controlando a sua rotação. Isto é feito fazendo com que os vetores camera_position_c e camera_lookat_l recebam as posições do gato, assim dependendo delas e tornando o centro da camera no gato.

Tranformações controladas pelo usuario:
  As tranformações controladas pelo usuario são feitas no arquivo main.cpp, mais precisamente utilizando da funçaõ keyCallBack e da main. As tranformações feitas são:
  W - movimenta o gato para frente
  S - movimenta o gato para trás
  space - faz o gato pular
  I - faz o gato retornar a uma posição especifica
  A camera lookat também é controlada pelo usuario ao segurar o botão esquerdo do mouse

  Quando o programa recebe um input de movimentação para frente ou para trás, ele seta uma flag(cat_walking_front ou cat_walking_back) como verdadeira, onde então na main é feita a movimentação do gato ao somar ou subtrair(dependendo do input W ou S) suas posições x e z com as posições da camera_view multiplicada por um delta_time(usado para que as movimentações sejam feitas por frame) e uma constante(para a velocidade)para que o gato então ande. Se o input recebido é o space, ele seta uma variavel chamada velocity_y(que começa em 0.0) com um valor constante, e na main este valor sempre esta sendo somado contantemente a posição y do gato e faz com que ele pule. Caso o gato ainda esteja "no ar", a velocidade é subtraida pela gravidade(uma constante) multiplicada pelo delta_time para fazer com que o gato desça. Se o input é I, ele simplesmente seta a posiçaõ do gato para uma posiçaõ especifica.

Colisão:
  As colisões implementadas são caixa-caixa e esfera-caixa. Para isso foram definidas bounding boxes para o gato, a cama e a esfera.

  Os bounding boxes do gato e da cama são da mesma estrutura, sendo apenas instanciados com valores referentes aos objetos que ele devem representar. Já a bounding box da esfera é mais complicada, precisando haver um calculo para reescalar e transladar a os valore min e max da bounding box dada pelo g_VirtualScene, a translação da esfera depende do vetor sphere_position para garantir que sua bounding box siga a posição da esfera quando ela se mover. A bounding box do gato também depende do vetor cat_position pelo mesmo motivo citado anteriormente.
  
  colisão caixa-caixa:
    A colisão caixa a caixa é feita detectando se a posição da do gato esta entre sua bounding box e a bounding box da cama. Quando o gato esta no chão e a colisão é detectada, ele simplesmente volta para uma posiçaõ especifica, não o deixando atravessar a cama. Caso a colisão seja detectada em cima da cama, a velocidade do pulo do gato é zerada, fazendo com que a gravidade n aja mais sobre ela e o gato possa se movimentar em cima da cama. 
    A colisão esfera-caixa é feita detectando se a distacia ao quadrado, que é calculada utilizando a parte da bounding box mais proxima do centro da esfera, é menor do que o raio ao quadrado. Quando isso acontece, o bool hitsphere fica como verdadeiro, assim confirmando a detecção da colisão.

Curva de Bezier:
  Ao colidir com a esfera, a curva de bezier que a movimente é ativada. Isto é dado pelo bool hitsphere, fazendo com que a variavel inicio pegue o tempo inicial e a variavel fim seja inicializada com o inicio + duration(que é uma constante para a duração da curva). Apos isso, as variaveis current_timee e interval são inicializadas com glfwGetTime e sin(current_timee*0.08) respectivamente. Caso o hitspshere seja verdadeiro ou o complete_move(se o movimento foi completado ou não) seja fals, ele então faz a posição da esfera receber as posições dadas pela curva de bezier, sendo mais rápida no inicio um pouco mais lenta ao longo da duração. Quando a posição da esfera atinge o ultimo ponto da curva de Bezier(P3), ele então seta o complete sphere como verdadeiro.
  


