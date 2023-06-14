# Plik źródłowy main.yml odpowiedzialny za uruchomienie Github Action
```
# Nazwa akcji
name: Budowa obrazu serwera i wysłanie do Docker Hub

# Akcja wykona się, gdy zajdą zmiany w głównym rozgałęzieniu, ale z wyjątkiem plików o rozszerzeniu .md (np: README.md)
on:
  push:
    branches:
      - "main"
    paths-ignore:
      - '**.md'

# Praca "zbuduj_serwer" i jej kroki Checkout, Ustawianie QEMU, Ustawianie Docker Buildx, Zaloguj się do Docker Hub, Zbuduj i wyślij
# Do zalogowania się w usłudze Docker Hub wykorzystuje zapisane sekrety DOCKERHUB_USERNAME i DOCKERHUB_TOKEN
# Nowa wersja oprogramowania jest wysyłana do publicznego repozytorium w usłudze Docker Hub
jobs:
  zbuduj_serwer:
    runs-on: ubuntu-latest
    steps:
      -
        name: Checkout
        uses: actions/checkout@v3
      -
        name: Ustawianie QEMU
        uses: docker/setup-qemu-action@v2
      -
        name: Ustawianie Docker Buildx
        uses: docker/setup-buildx-action@v2
      -
        name: Zaloguj się do Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      -
        name: Zbuduj i wyślij
        uses: docker/build-push-action@v4
        with:
          context: .
          platforms: linux/amd64,linux/arm64/v8
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/test:latest
```
