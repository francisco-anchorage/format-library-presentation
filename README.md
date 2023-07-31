# format-library-presentation
This repo contains the presentation and code examples of the Jul 31st 2023 C++ Study Session

## How to build the slides?

```
# Convert slide deck into HTML
docker run --rm --init -v $PWD:/home/marp/app/ -e MARP_USER="$(id -u):$(id -g)" -e LANG=$LANG marpteam/marp-cli --allow-local-files PITCHME.md -o index.html

# Convert slide deck into PDF
docker run --rm --init -v $PWD:/home/marp/app/ -e MARP_USER="$(id -u):$(id -g)" -e LANG=$LANG marpteam/marp-cli --allow-local-files PITCHME.md -o presentation.pdf

# Server mode (Serve current directory in http://localhost:8080/)
docker run --rm --init -v $PWD:/home/marp/app -e LANG=$LANG -p 8080:8080 -p 37717:37717 marpteam/marp-cli --server .
```