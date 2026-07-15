# Guia de Importação de Bibliotecas no KiCad

Este tutorial rápido ensina como adicionar bibliotecas baixadas do GitHub e importar arquivos do Eagle para o seu projeto no KiCad.

## 1. Importando Bibliotecas Nativas (GitHub)

Siga estes passos para bibliotecas com o formato padrão do KiCad:

1. Faça o `git clone` do repositório no local de sua preferência.
   > **Dica:** Crie uma pasta separada para o KiCad e outra específica para as bibliotecas. Isso facilita muito na hora de baixar as atualizações constantes.
2. Abra o **Editor de Símbolos**.
3. Acesse **Arquivo > Adicionar Biblioteca...**.
4. Selecione os arquivos com a extensão `.kicad_sym`.
5. Salve as alterações. Feche o editor de símbolos.
6. Abra o **Editor de Footprints**.
7. Acesse **Preferências > Gerenciar Bibliotecas de Footprint...**.
8. Clique no ícone de pasta, localizado próximo ao canto inferior esquerdo.
9. Selecione as pastas com a extensão `.pretty`. Isso vincula diretamente cada footprint ao seu respectivo símbolo. Pronto, importação concluída!

---

## 2. Importando Bibliotecas do Eagle

Caso precise utilizar componentes direto de uma biblioteca do Eagle, o processo é um pouco diferente:

1. Crie uma nova biblioteca dentro do KiCad.
2. Clique com o botão direito na nova biblioteca. Selecione **"Importar símbolo..."**.
3. Selecione o arquivo `.lbr` do Eagle.
4. Feche o **Editor de Símbolos**. Abra o **Editor de Footprints**.
5. Acesse **Preferências > Gerenciar Bibliotecas de Footprint...**.
6. Clique no ícone imediatamente à direita do ícone de pasta (canto inferior esquerdo).
7. Defina o tipo de arquivo a ser buscado. Selecione **Eagle (*.lbr)**.
8. Navegue até o local do arquivo do Eagle e selecione-o. Biblioteca de footprints importada!

> **Atenção:** As bibliotecas de footprint importadas do Eagle operam em modo **somente leitura**. Para adicionar novos componentes posteriormente, será necessário criar uma nova biblioteca exclusiva para essa finalidade.