# Plik źródłowy main.yml odpowiedzialny za uruchomienie Github Action
```
name: Budowa obrazu serwera i wysłanie do Docker Hub

on:
  push:
    branches:
      - "main"
    paths-ignore:
      - '**.md'

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
## Nazwa akcji
```
name: Budowa obrazu serwera i wysłanie do Docker Hub
```
## Akcja wykona się, gdy zajdą zmiany w głównym rozgałęzieniu, ale z wyjątkiem plików o rozszerzeniu .md (np: README.md)
```
on:
  push:
    branches:
      - "main"
    paths-ignore:
      - '**.md'
```
## Praca "zbuduj_serwer"
```
jobs:
  zbuduj_serwer:
    runs-on: ubuntu-latest
```
## Poszczególne kroki "zbuduj_serwer"
### Checkout
```
    steps:
      -
        name: Checkout
        uses: actions/checkout@v3
```
### Ustawianie QEMU
```
      -
        name: Ustawianie QEMU
        uses: docker/setup-qemu-action@v2
```
### Ustawianie Docker Buildx
```
      -
        name: Ustawianie Docker Buildx
        uses: docker/setup-buildx-action@v2
```
### Zalogowanie się do Docker Hub
#### Przy logowaniu zostały wykorzystyne zapisane sekrety DOCKERHUB_USERNAME i DOCKERHUB_TOKEN
```
      -
        name: Zaloguj się do Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
```
### Zbuduj i wyślij obraz do Docker Hub
#### Nowa wersja oprogramowania (przeznaczona do architektur linux/amd64 oraz linux/arm64/v8) jest wysyłana do publicznego repozytorium w usłudze Docker Hub
#### Aby wysłać obraz do poprawnego repozytorium ponownie wykorzystujemy sekret DOCKERHUB_USERNAME do określenia ścieżki
```
      -
        name: Zbuduj i wyślij
        uses: docker/build-push-action@v4
        with:
          context: .
          platforms: linux/amd64,linux/arm64/v8
          push: true
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/test:latest
```
# Udane automatyczne próby wykonania akcji
![image](https://github.com/INeedEstus/docker_laboratorium2/assets/79727495/2eed3e39-446b-400c-8c2b-3d45ffc04869)
# Podsumowanie najnowszej udanej próby, wyświetlona jest pojedyńcza praca "zbuduj_serwer"
![image](https://github.com/INeedEstus/docker_laboratorium2/assets/79727495/59ab4f26-fff0-48f3-9d28-1dbbde21b153)
# Poszczególne wykonane kroki i ich logi
![image](https://github.com/INeedEstus/docker_laboratorium2/assets/79727495/6043fcfd-a27a-4eae-a022-cd4a23644091)
