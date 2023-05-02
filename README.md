[![Container build](https://github.com/digitaltom/semantic-knowledge-search/actions/workflows/docker-publish.yml/badge.svg)](https://github.com/digitaltom/semantic-knowledge-search/pkgs/container/knowledge)

# Semantic knowledge search

Search engine to find related articles based on openai embedding, and answer with GPT. An installation with SUSE documentation + knowlwdge base articles as
data source is available at: https://geeko.port0.org/

![2023-04-13_12-04](https://user-images.githubusercontent.com/582520/231726466-d4e54b1d-4c8b-4a33-9596-e8d27cadbfd3.png)

To use the [SQLite VSS](https://github.com/asg017/sqlite-vss) (SQLite Vector Similarity Search) extension, you need to install the packages `libgomp1, libblas3, liblapack3`.

- Import articles with and create openai embedding vectors with:
  - `rake log:info import:doc` (import all doc pages)
  - `rake log:info import:doc[url]` (import page from url)
  - `rake log:info import:kb` (import knowledge base articles)
  - `Article.vectorize_all(reindex: false)` (vectorize articles)
- Find relevant articles with: `Question.new("question text").related_articles`
- Create answer with: `Answer.new(question, article).generate`

### Development

To run the app locally, `cssbundling-rails` requires to run `yarn build:css --watch` in a second terminal next to `rails s`. For `jsbundling-rails`, you need to do the same with `yarn build --watch`. Those commands are combined in `Procfile.dev` and can be run with `foreman start -f Procfile.dev`.

* Build image: `DOCKER_BUILDKIT=1 docker build -t ghcr.io/digitaltom/semantic-knowledge-search .`
* Run image: `docker run -ti --network=host --rm -e SECRET_KEY_BASE=<random_secret_key> -e OPENAI_API_KEY=<key> -v <path to production.sqlite3>:/rails/db/production.sqlite3 -name knowledge ghcr.io/digitaltom/semantic-knowledge-search`
* Exec into container: `docker exec knowledge /bin/bash`

### Related articles

* https://simonwillison.net/2023/Jan/13/semantic-search-answers/
* [Metadata filtering in vector similarity search](https://www.pinecone.io/learn/vector-search-filtering/)

