# 🔍 Guia de Sobrevivência: 40 Hacks do Comando FIND

O comando `find` é a ferramenta definitiva para localizar arquivos e diretórios com base em metadados (tempo, tamanho, permissões) e executar ações automáticas sobre eles.

---

## 📂 1. Busca por Nome e Extensão
1. **Busca simples por nome:** `find . -name "arquivo.txt"`
2. **Ignorar maiúsculas/minúsculas:** `find . -iname "FOTO.JPG"`
3. **Buscar todos os arquivos de uma extensão:** `find . -type f -name "*.md"`
4. **Buscar arquivos que NÃO possuem a extensão:** `find . -type f ! -name "*.log"`
5. **Buscar múltiplos padrões (OR):** `find . \( -name "*.sh" -o -name "*.py" \)`
6. **Buscar arquivos que começam com "vendas":** `find . -name "vendas*"`

## ⚖️ 2. Filtros por Tamanho
7. **Arquivos maiores que 100MB:** `find / -size +100M`
8. **Arquivos menores que 1k:** `find . -size -1k`
9. **Arquivos entre 10MB e 50MB:** `find . -size +10M -size -50M`
10. **Arquivos exatamente de um tamanho:** `find . -size 10k`
11. **Arquivos vazios (0 bytes):** `find . -empty`

## 📅 3. Filtros por Tempo (Modificação e Acesso)
12. **Modificados nas últimas 24 horas:** `find . -mtime 0`
13. **Modificados nos últimos 7 dias:** `find . -mtime -7`
14. **Modificados há mais de 30 dias:** `find . -mtime +30`
15. **Acessados nos últimos 10 minutos:** `find . -amin -10`
16. **Alterados após a criação de um arquivo referência:** `find . -newer referencia.txt`
17. **Modificados nos últimos 60 minutos:** `find . -mmin -60`

## 🌳 4. Escopo e Tipo de Arquivo
18. **Apenas arquivos comuns:** `find . -type f`
19. **Apenas diretórios:** `find . -type d`
20. **Apenas links simbólicos:** `find . -type l`
21. **Limitar busca à pasta atual (sem subpastas):** `find . -maxdepth 1 -name "*.php"`
22. **Buscar apenas em subpastas profundas:** `find . -mindepth 2 -maxdepth 5`

## 🛡️ 5. Permissões e Usuários
23. **Arquivos com permissão exata 777:** `find . -perm 777`
24. **Arquivos com permissão de execução:** `find . -perm /a+x`
25. **Arquivos de um usuário específico:** `find /home -user nome_usuario`
26. **Arquivos de um grupo específico:** `find /var -group www-data`
27. **Arquivos sem dono (órfãos):** `find / -nouser`

## ⚡ 6. Executando Ações (O "Pulo do Gato")
28. **Listar detalhes (ls -l) dos achados:** `find . -name "*.log" -ls`
