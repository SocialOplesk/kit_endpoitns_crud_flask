# SOCIAL OPLESK
### 
<br/>

# Guía Rápida del funcionamiento de los endpoints
Esta guía proporciona pasos simples y ejemplos concretos para quienes comienzan a crear Endpoints.


## 1. query, params, body
```
from flask import Flask, request, jsonify
from flask_cors import CORS
import requests

app = Flask(__name__)
CORS(app)


# 1. Recibir "foo" como string desde query params
@app.route('/by_query', methods=['GET'])
def by_query():
    foo = request.args.get("foo", type=str)
    return jsonify({"received_from_query": foo})

# 2. Recibir "foo" (str) y "bar" (int) como path params
@app.route('/by_params/<string:foo>/<int:bar>', methods=['GET'])
def by_params(foo, bar):
    return jsonify({"received_from_params": {"foo": foo, "bar": bar}})

# 3. Recibir "foo" (str) y "bar" (int) en el body (JSON)
@app.route('/by_body', methods=['POST'])
def by_body():
    data = request.get_json()
    foo = data.get("foo")
    bar = data.get("bar")
    return jsonify({"received_from_body": {"foo": foo, "bar": bar}})


if __name__ == "__main__":
    app.run(debug=True)
```


## 2. CRUD

```
from flask import Flask, request, jsonify
from flask_cors import CORS
import requests

app = Flask(__name__)
CORS(app)

# 1.  CREATE
@app.route('/items', methods=['POST'])
def create_item():
    data = request.get_json()
    items.append(data)
    return jsonify({"added": data, "all_items": items}), 201

# 2.   READ
@app.route('/items', methods=['GET'])
def get_items():
    return jsonify({"all_items": items})

# 3.  UPDATE
@app.route('/items/<int:item_idx>', methods=['PUT'])
def update_item(item_idx):
    if item_idx < 0 or item_idx >= len(items):
        return jsonify({"error": "Item does not exist"}), 404
    data = request.get_json()
    items[item_idx] = data
    return jsonify({"updated_item": data, "all_items": items})

# 4.  DELETE
@app.route('/items/<int:item_idx>', methods=['DELETE'])
def delete_item(item_idx):
    if item_idx < 0 or item_idx >= len(items):
        return jsonify({"error": "Item does not exist"}), 404
    removed = items.pop(item_idx)
    return jsonify({"removed": removed, "all_items": items})


if __name__ == "__main__":
    app.run(debug=True)
```


## 3. JSONPLACEHOLDER

```
from flask import Flask, request, jsonify
from flask_cors import CORS
import requests

app = Flask(__name__)
CORS(app)

items = []


# 1. GET a jsonplaceholder (usando requests)
@app.route('/ext/posts', methods=['GET'])
def get_external_posts():
    resp = requests.get('https://jsonplaceholder.typicode.com/posts')
    return jsonify(resp.json()), resp.status_code

# 2. POST a jsonplaceholder (usando requests)
@app.route('/ext/posts', methods=['POST'])
def post_external_post():
    data = request.get_json()
    resp = requests.post('https://jsonplaceholder.typicode.com/posts', json=data)
    return jsonify(resp.json()), resp.status_code


if __name__ == "__main__":
    app.run(debug=True)
```
