# Recomendador de Músicas YouTube Music em Streamlit

Aplicação web em Python/Streamlit que busca músicas e vídeos no YouTube, permite favoritar faixas e gera recomendações com base em histórico de buscas e playlist de favoritos, usando TF‑IDF, similaridade do cosseno, similaridade de Jaccard e um score híbrido.

## Visão geral
- Busca vídeos do YouTube (YouTube Data API v3) a partir de artista, música ou gênero.
- Exibe cards com capa, título, canal, descrição, player embutido e métricas.
- Permite favoritar vídeos e montar uma playlist exportável em CSV.
- Calcula recomendações com base na busca atual ou nas músicas já favoritedas.
- Traz um dashboard com gráficos dos scores de similaridade.
- Possui modo demonstração (offline), sem chamada à API.

## Funcionalidades principais
- Campo de busca por texto (artista, música, gênero etc.).
- Filtros na sidebar: intervalo de ano de publicação; quantidade de resultados; ordenação (mais novo, mais antigo, mais visualizações, mais curtidas); duração (até 4 min, 4–10 min, mais de 10 min); opção “apenas com descrição”.
- Modo demonstração (offline), usando um conjunto fixo de músicas brasileiras.
- Aba Busca: lista de resultados; recomendações com score híbrido; player de vídeo embutido; botão de “Favoritar” e seção “Playlist Favoritos” com download em CSV.
- Aba Dashboard de Gráficos: gráfico de barras do score híbrido; gráfico de linha/dispersão do score cosseno.
- Aba Explicação do Algoritmo: tokens da busca/histórico; vocabulário do TF‑IDF; primeiros valores do vetor TF‑IDF; comparação de scores (cosseno, Jaccard, híbrido).

## Como funciona o algoritmo de recomendação
1. Pré‑processamento de texto: converte tudo para minúsculas; remove acentos e pontuação; remove stopwords em português (NLTK); junta título, canal e descrição para cada vídeo.
2. Representação com TF‑IDF: usa TfidfVectorizer com ngram_range=(1, 2); gera a matriz TF‑IDF para todos os vídeos encontrados; gera o vetor TF‑IDF para o histórico de favoritos ou para a última consulta do usuário (se não houver favoritos).
3. Cálculo de similaridade: similaridade do cosseno entre o vetor do histórico e a matriz TF‑IDF dos vídeos; similaridade de Jaccard entre tokens do histórico e de cada vídeo; score híbrido como max(score_cosseno, score_jaccard) para cada vídeo; bônus se parte do texto do histórico aparecer diretamente no título/canal (score = 1.0).
4. Ordenação: ordena os vídeos pelo maior score híbrido; exibe as top 5 recomendações com score híbrido, score cosseno, score Jaccard e player do vídeo.

## Tecnologias usadas
- Python 3.x
- Streamlit
- Pandas, NumPy
- scikit‑learn (TfidfVectorizer, cosine_similarity)
- NLTK (stopwords em português)
- Plotly (express e graph_objects)
- Google API Client (YouTube Data API v3)
- isodate
- unicodedata, re

## Instalação
1. Crie uma pasta para o projeto e coloque dentro o arquivo .py do app Streamlit e este README.md.
2. (Opcional, recomendado) Crie um ambiente virtual:
   - python -m venv .venv
   - Ativar no Windows: .venv\Scripts\activate
   - Ativar no Linux/Mac: source .venv/bin/activate
3. Instale as dependências:
   - pip install streamlit pandas numpy nltk isodate google-api-python-client scikit-learn plotly
4. Baixe as stopwords em português (uma vez só, no Python):
   - import nltk
   - nltk.download('stopwords')

## Configurando a chave da YouTube API
No código existe uma variável: API_KEY = 'SUA_CHAVE_AQUI'

Passos:
1. Crie um projeto no Google Cloud Console.
2. Ative a YouTube Data API v3.
3. Crie uma chave de API.
4. Substitua a string da API_KEY pela sua chave.

Importante: se o repositório for público, evite subir a chave real. Em produção, use variáveis de ambiente ou arquivos ignorados pelo .gitignore.

## Como rodar o app
Na pasta do projeto, execute: streamlit run nome_do_arquivo.py

O navegador deve abrir automaticamente em http://localhost:8501 com a interface do app.

## Como usar
- Digite uma consulta (ex.: “Gusttavo Lima sertanejo”) e clique em “Buscar músicas/vídeos 🔎”.
- Ajuste os filtros na sidebar (ano, duração, ordenação, etc.).
- Role a página para ver recomendações com scores, cards de vídeos e métricas de visualizações e likes.
- Clique em Favoritar para adicionar um vídeo à playlist.
- Use a seção “Playlist Favoritos” para ver os itens favoritados e baixar um CSV com a playlist.
- Ative Modo demonstração (offline) na sidebar para testar o algoritmo sem chamar a API real.

## Possíveis melhorias
- Persistir favoritos em banco de dados ou arquivo local.
- Autenticação de usuário e histórico separado por usuário.
- Ajustar pesos entre cosseno e Jaccard (combinação ponderada em vez de máximo).
- Interface mais rica com capas customizadas e mais filtros.
- Deploy em Streamlit Cloud ou outra plataforma.

## Licença
Defina aqui a licença do projeto (por exemplo, MIT, Apache 2.0, etc.).
