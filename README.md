# Projeção Ortográfica Frontal com OpenGL Moderno

Este projeto apresenta um exemplo didático de renderização de um cubo 3D colorido utilizando OpenGL moderno. O objetivo principal é demonstrar, de forma prática, como uma matriz homogênea pode ser usada para produzir uma vista ortográfica frontal.

## Objetivo do código

O código renderiza um cubo 3D e aplica uma matriz de projeção frontal que remove a coordenada `z` dos vértices. Com isso, o cubo é projetado no plano `xy`, produzindo uma vista frontal.

A transformação aplicada é:

```text
(x, y, z) -> (x, y, 0)
```

A matriz utilizada é:

```text
[1 0 0 0]
[0 1 0 0]
[0 0 0 0]
[0 0 0 1]
```

No código, essa matriz é montada com GLM da seguinte forma:

```cpp
glm::mat4 model = glm::mat4(0.0f);

model[0][0] = 1.0f;
model[1][1] = 1.0f;
model[3][3] = 1.0f;
```

## Bibliotecas utilizadas

O código utiliza:

- GLAD: carregamento das funções OpenGL;
- GLFW: criação da janela e contexto OpenGL;
- GLM: manipulação de matrizes e vetores;
- OpenGL 3.3 Core Profile.

## Estrutura dos vértices

Cada vértice do cubo possui 6 valores:

```text
x, y, z, r, g, b
```

onde:

- `x, y, z`: posição do vértice no espaço 3D;
- `r, g, b`: cor do vértice.

Exemplo:

```cpp
-0.5f, -0.5f, -0.5f,   1.0f, 0.0f, 0.0f
```

Esse vértice possui posição `(-0.5, -0.5, -0.5)` e cor vermelha `(1.0, 0.0, 0.0)`.

## Configuração dos atributos

O atributo de posição é configurado em `location = 0`:

```cpp
glVertexAttribPointer(
    0,
    3,
    GL_FLOAT,
    GL_FALSE,
    6 * sizeof(float),
    (void*)0
);
```

O atributo de cor é configurado em `location = 1`:

```cpp
glVertexAttribPointer(
    1,
    3,
    GL_FLOAT,
    GL_FALSE,
    6 * sizeof(float),
    (void*)(3 * sizeof(float))
);
```

## Shaders

O código usa shaders escritos diretamente no arquivo C++.

### Vertex Shader

O vertex shader recebe a posição e a cor de cada vértice. A posição é transformada pela matriz `model`:

```glsl
gl_Position = model * vec4(aPos, 1.0);
```

### Fragment Shader

O fragment shader apenas aplica a cor recebida do vertex shader:

```glsl
FragColor = vec4(vertexColor, 1.0);
```

## Como compilar

A forma de compilação depende da estrutura do projeto e da instalação local das bibliotecas.

Um exemplo simplificado usando `g++` seria:

```bash
g++ main.cpp glad.c -o projecao \
    -lglfw -ldl -lGL
```

Em projetos com CMake, recomenda-se configurar corretamente os diretórios de inclusão e linkar as bibliotecas `glfw`, `glad` e `OpenGL`.

## Resultado esperado

Ao executar o programa, será exibida uma vista frontal do cubo. Como a coordenada `z` foi eliminada, a profundidade deixa de influenciar a visualização e o cubo passa a se comportar visualmente como uma projeção no plano `xy`.

## Experimentos sugeridos

### 1. Vista frontal

Matriz atual:

```cpp
model[0][0] = 1.0f;
model[1][1] = 1.0f;
model[3][3] = 1.0f;
```

Transformação:

```text
(x, y, z) -> (x, y, 0)
```

### 2. Vista superior

Altere para:

```cpp
glm::mat4 model = glm::mat4(0.0f);

model[0][0] = 1.0f;
model[2][1] = 1.0f;
model[3][3] = 1.0f;
```

Transformação:

```text
(x, y, z) -> (x, z, 0)
```

Como a tela utiliza os eixos `x` e `y`, o valor original de `z` é remapeado para o eixo vertical da tela.

### 3. Vista lateral

Altere para:

```cpp
glm::mat4 model = glm::mat4(0.0f);

model[2][0] = 1.0f;
model[1][1] = 1.0f;
model[3][3] = 1.0f;
```

Transformação:

```text
(x, y, z) -> (z, y, 0)
```

Nesse caso, a coordenada `x` deixa de influenciar a visualização.

## Observação sobre o GLM

No GLM, o acesso aos elementos da matriz é feito no formato:

```cpp
matriz[coluna][linha]
```

Por isso, é mais seguro montar as matrizes usando atribuições diretas, como:

```cpp
model[0][0] = 1.0f;
model[1][1] = 1.0f;
```

Esse formato evita confusão com a organização interna das matrizes em memória.

## Questões para reflexão

1. Qual coordenada é eliminada na vista frontal?
2. Em qual plano ocorre a projeção frontal?
3. Por que o cubo passa a parecer um quadrado?
4. Qual a diferença entre vista frontal, superior e lateral?
5. Por que projeções ortográficas não possuem ponto de fuga?
6. Qual é o papel da matriz homogênea nesse experimento?

## Observação didática

Este exemplo não utiliza textura, iluminação ou câmera. A intenção é isolar o efeito da matriz homogênea de projeção, permitindo que o aluno observe diretamente a relação entre álgebra linear e visualização 3D.
