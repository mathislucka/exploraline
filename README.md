# explorAline

explorAline is a Vue-based tool to visually inspect textual data in json-line files.

## features

- load and parse .jsonl files
- select props that should be displayed
- color coding of items based on their content
- use python list slicing syntax to paginate through your file
- basic search through displayed properties


## usage

```
git clone https://github.com/mathislucka/exploraline.git
cd exploraline

# if you have npm installed
npm  install
npm run dev
# navigate to localhost:3000


# if you prefer to use a python webserver
cd dist
python3 -m http.server
# navigate to localhost:8000


# select a jsonl-file and select the props you'd like to display
# explore a line

```
