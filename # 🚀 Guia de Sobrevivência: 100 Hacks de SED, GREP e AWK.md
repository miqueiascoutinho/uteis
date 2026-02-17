# 🚀 Guia de Sobrevivência: 100 Hacks de SED, GREP e AWK

Este guia compila comandos essenciais para manipulação de texto em sistemas Unix/Linux.

---

## 🛠️ PARTE 1: GREP (Pesquisa e Filtros) - [33 Hacks]

O `grep` é sua lupa. Ele foca em encontrar padrões.

### Básico e Essencial
1. **Busca simples:** `grep "padrao" arquivo.txt`
2. **Ignorar maiúsculas/minúsculas:** `grep -i "padrao" arquivo.txt`
3. **Contar ocorrências:** `grep -c "erro" servidor.log`
4. **Mostrar números das linhas:** `grep -n "config" setup.sh`
5. **Inverter busca (o que NÃO tem o padrão):** `grep -v "debug" app.log`
6. **Buscar em todos os arquivos do diretório:** `grep "query" *`
7. **Busca recursiva em subpastas:** `grep -r "function" ./src`
8. **Listar apenas nomes de arquivos com o padrão:** `grep -l "TODO" *.md`
9. **Listar arquivos que NÃO contêm o padrão:** `grep -L "copyright" *.py`

### Filtros Avançados
10. **Palavra exata (não parte de outra):** `grep -w "python" script.py`
11. **Linhas que começam com algo:** `grep "^Inicio" texto.txt`
12. **Linhas que terminam com algo:** `grep "Fim$" texto.txt`
13. **Mostrar X linhas após o match:** `grep -A 3 "erro" log.txt`
14. **Mostrar X linhas antes do match:** `grep -B 2 "sucesso" log.txt`
15. **Contexto (antes e depois):** `grep -C 2 "panic" kernel.log`
16. **Expressões regulares estendidas:** `grep -E "erro|falha|critico" log.txt`
17. **Buscar linhas vazias:** `grep "^$" arquivo.txt`
18. **Remover linhas vazias do output:** `grep -v "^$" arquivo.txt`

### Hacks de Produtividade
19. **Destacar cores no output:** `grep --color=auto "destaque" arquivo.txt`
20. **Buscar múltiplos padrões (OR):** `grep -e "warn" -e "error" log.txt`
21. **Extrair apenas a parte casada (não a linha toda):** `grep -o "ID-[0-9]*" pedidos.csv`
22. **Ler padrões de um arquivo:** `grep -f lista_de_erros.txt log_do_dia.txt`
23. **Ignorar binários:** `grep -I "texto" *`
24. **Parar após X ocorrências:** `grep -m 5 "tentativa" acesso.log`
25. **Combinar com pipes:** `ls -l | grep ".pdf$"`
26. **Busca silenciosa (bom para scripts):** `grep -q "admin" /etc/passwd && echo "Existe"`
27. **Encontrar IPs (Regex simples):** `grep -E -o "([0-9]{1,3}\.){3}[0-9]{1,3}" access.log`
28. **Buscar caracteres não-ASCII:** `grep -P "[^\x00-\x7f]" arquivo.txt`
29. **Grep em arquivos compactados:** `zgrep "erro" log.gz`
30. **Exibir nome do arquivo em cada linha:** `grep -H "pattern" *`
31. **Excluir diretórios na busca:** `grep -r "main" . --exclude-dir={node_modules,.git}`
32. **Excluir arquivos específicos:** `grep -r "main" . --exclude=*.min.js`
33. **Contar linhas que não são comentários:** `grep -cv "^#" config.conf`

---

## 🖋️ PARTE 2: SED (Editor de Fluxo) - [33 Hacks]

O `sed` é o seu bisturi. Use-o para transformar e substituir.

### Substituições
34. **Trocar primeira ocorrência por linha:** `sed 's/antigo/novo/' arq.txt`
35. **Trocar todas as ocorrências (Global):** `sed 's/antigo/novo/g' arq.txt`
36. **Trocar apenas na linha 5:** `sed '5s/antigo/novo/' arq.txt`
37. **Trocar da linha 10 até a 20:** `sed '10,20s/antigo/novo/g' arq.txt`
38. **Editar o arquivo original (In-place):** `sed -i 's/fix/done/g' arq.txt`
39. **Apagar linhas que contêm padrão:** `sed '/remover/d' arq.txt`
40. **Apagar linhas vazias:** `sed '/^$/d' arq.txt`
41. **Apagar as primeiras 3 linhas:** `sed '1,3d' arq.txt`
42. **Apagar da linha 5 até o final:** `sed '5,$d' arq.txt`

### Manipulação de Linhas
43. **Inserir linha antes de um match:** `sed '/padrao/i Novo Texto' arq.txt`
44. **Adicionar linha após um match:** `sed '/padrao/a Texto Depois' arq.txt`
45. **Substituir a linha inteira que tem o padrão:** `sed '/erro/c Linha Corrigida' arq.txt`
46. **Imprimir apenas linhas específicas (ex: 5 a 8):** `sed -n '5,8p' arq.txt`
47. **Converter tudo para maiúsculas (GNU sed):** `sed 's/.*/\U&/' arq.txt`
48. **Inverter ordem das linhas (tipo tac):** `sed '1!G;h;$!d' arq.txt`
49. **Remover espaços no final das linhas:** `sed 's/[[:space:]]*$//' arq.txt`
50. **Adicionar prefixo em todas as linhas:** `sed 's/^/PREFIXO_/' arq.txt`
51. **Extrair extensão de arquivo:** `echo "foto.jpg" | sed 's/.*\.//'`

### Hacks Avançados
52. **Substituir apenas a N-ésima ocorrência:** `sed 's/azul/verde/2' cores.txt`
53. **Executar múltiplos comandos:** `sed -e 's/a/b/' -e 's/c/d/' arq.txt`
54. **Trocar delimitador (útil para caminhos/URLs):** `sed 's|/usr/bin|/usr/local/bin|' arq.txt`
55. **Imprimir apenas o que foi alterado:** `sed -n 's/erro/FIX/p' log.txt`
56. **Remover tags HTML:** `sed 's/<[^>]*>//g' index.html`
57. **Adicionar parênteses em volta de números:** `sed 's/[0-9]*/(&)/g' arq.txt`
58. **Pegar a última linha:** `sed -n '$p' arq.txt`
59. **Remover comentários (#):** `sed 's/#.*//' config.py`
60. **Duplicar cada linha:** `sed 'p' arq.txt`
61. **Mesclar linhas (join):** `sed ':a;N;$!ba;s/\n/ /g' arq.txt`
62. **Remover caracteres não-imprimíveis:** `sed 's/[^[:print:]]//g' arq.txt`
63. **Trocar apenas a última ocorrência de uma linha:** `sed 's/\(.*\)./\1!/' arq.txt`
64. **Converter Windows Newlines (CRLF) para Unix:** `sed 's/\r$//' script.sh`
65. **Numerar linhas (estilo cat -n):** `sed = arq.txt | sed 'N;s/\n/\t/'`
66. **Remover linhas duplicadas consecutivas:** `sed '$!N; /^\(.*\)\n\1$/!P; D' arq.txt`

---

## 📊 PARTE 3: AWK (Processamento de Dados) - [34 Hacks]

O `awk` é sua planilha no terminal. Excelente para colunas e cálculos.

### Colunas e Delimitadores
67. **Imprimir primeira coluna:** `awk '{print $1}' dados.txt`
68. **Imprimir última coluna:** `awk '{print $NF}' dados.txt`
69. **Imprimir penúltima coluna:** `awk '{print $(NF-1)}' dados.txt`
70. **Mudar delimitador para vírgula (CSV):** `awk -F"," '{print $2}' base.csv`
71. **Imprimir múltiplas colunas com texto:** `awk '{print "Usuário: " $1 "\t ID: " $3}' /etc/passwd`
72. **Verificar se coluna 3 é maior que 100:** `awk '$3 > 100' vendas.txt`
73. **Somar valores da coluna 1:** `awk '{sum += $1} END {print sum}' numeros.txt`

### Filtros Lógicos
74. **Filtrar por texto em coluna específica:** `awk '$2 == "ERRO"' log.txt`
75. **Filtrar por Regex em coluna:** `awk '$1 ~ /^192\./' access.log`
76. **Contar linhas do arquivo:** `awk 'END {print NR}' arq.txt`
77. **Calcular média da coluna 2:** `awk '{sum += $2} END {print sum/NR}' dados.txt`
78. **Imprimir linhas com mais de 50 caracteres:** `awk 'length($0) > 50' arq.txt`
79. **Imprimir linhas que contêm "admin" na coluna 1 ou 2:** `awk '$1 ~ /admin/ || $2 ~ /admin/' users.txt`

### Transformação de Dados
80. **Trocar ordem de colunas (1 e 2):** `awk '{tmp=$1; $1=$2; $2=tmp; print}' arq.txt`
81. **Remover linhas duplicadas (sem sort):** `awk '!visited[$0]++' grande.txt`
82. **Imprimir linhas entre 10 e 20:** `awk 'NR==10,NR==20' arq.txt`
83. **Adicionar cabeçalho ao output:** `awk 'BEGIN {print "NOME,DATA"} {print $1 "," $2}' dados.txt`
84. **Buscar o maior valor na coluna 1:** `awk '$1 > max {max=$1} END {print max}' lista.txt`
85. **Extrair nomes de usuários (do /etc/passwd):** `awk -F: '{print $1}' /etc/passwd`
86. **Imprimir linhas onde a coluna 2 é única:** `awk '!a[$2]++' arquivo.txt`
87. **Inverter uma string (por linha):** `awk '{for(i=length;i!=0;i--)printf "%c",substr($0,i,1); printf "\n"}'`

### Hacks de Sistema e Scripts
88. **Mostrar uso de disco acima de 80%:** `df -h | awk '0+$5 > 80'`
89. **Matar processos por nome (ex: chrome):** `ps aux | grep chrome | awk '{print $2}' | xargs kill`
90. **Contar frequência de palavras:** `awk '{for(i=1;i<=NF;i++) count[$i]++} END {for(w in count) print w, count[w]}' texto.txt`
91. **Formatar output com colunas alinhadas:** `awk '{printf "%-10s %-5d\n", $1, $2}' dados.txt`
92. **Extrair IPs únicos de um log:** `awk '{print $1}' access.log | sort | uniq`
93. **Substituir texto apenas em uma coluna:** `awk '{gsub(/antigo/, "novo", $3); print}' dados.txt`
94. **Imprimir a cada 5 linhas:** `awk 'NR % 5 == 0' log.txt`
95. **Soma de bytes em um diretório:** `ls -l | awk '{sum += $5} END {print sum}'`
96. **Validar quantidade de campos por linha:** `awk 'NF != 4 {print "Erro na linha " NR}' csv_de_4_colunas.csv`
97. **Imprimir apenas linhas ímpares:** `awk 'NR % 2 == 1' arquivo.txt`
98. **Transformar CSV em JSON simples:** `awk -F, '{print "{\"id\":\""$1"\", \"val\":\""$2"\"}"}' dados.csv`
99. **Encontrar a linha mais longa:** `awk '{ if (length($0) > max) {max = length($0); line = $0} } END { print line }' arq.txt`
100. **Remover espaços extras entre palavras:** `awk '{$1=$1; print}' texto_sujo.txt`
