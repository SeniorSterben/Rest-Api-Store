# Rest-Api
Las API son conjuntos de definiciones y protocolos que se utilizan para diseñar e integrar el software de las aplicaciones.Suele considerarse como el contrato entre el proveedor de información y el usuario, donde se establece el contenido que se necesita por parte del consumidor (la llamada) y el que requiere el productor (la respuesta).
Lo primero es crear una base de datos la cual vamos a recopilar con el método get y post.

Creación de los items de la Store:
stores = [
    {
        "name": "My Store",
        "items": [
            {
                "name": "Chair",
                "price": 15.99
            }
        ]
    }
]

Ahora creamos el servidor local:

from flask import Flask, jsonify

app = Flask(__name__) 

SE ESTABLECE UN ENDPOINT PARA ACCEDER AL CONTENIDO DEL SERVIDOR.
A CONTINUACIÓN SE USA EL MÉTODO GET PARA TRAER LA INFORMACIÓN.
@app.get("/store")
def get_stores():
    return {"stores": stores}

AHORA SE USA EL MÉTODO POST:
@app.post("/store")
def create_store():
    request_data = request.get_json()
    new_store = {"name": request_data["name"], "items": []}
    stores.append(new_store)
    return new_store, 201

AHORA SE CREA UN NUEVO ENDPOINT PARA AGREGAR OBJETOS A LA COLECCIÓN:
@app.post("/store/<string:name>/item")
def create_item(name):
    request_data = request.get_json()
    for store in stores:
        if store["name"] == name:
            new_item = {"name": request_data["name"], "price": request_data["price"]}
            store["items"].append(new_item)
            return new_item, 201
    return {"message": "Store not found"}, 404

UN ENDPOINT QUE NOS MUESTRE EL CONTENIDO DE UN DOCUMENTO CON SUS ARTICULOS:
@app.get("/store/<string:name>")
def get_store(name):
    for store in stores:
        if store["name"] == name:
            return store
    return {"message": "Store not found"}, 404

ESTE ENDPOINT NOS PERMITE VER LOS ARTICULOS DE UNA COLECCIÓN.
@app.get("/store/<string:name>/item")
def get_item_in_store(name):
    for store in stores:
        if store["name"] == name:
            return {"items": store["items"]}
    return {"message": "Store not found"}, 404
    
 CONDICIÓN PARA GUARDAR CAMBIOS EN EL SERVIDOR
if __name__ == '__main__':
    app.run(debug=True, port=4000)

Con este código hacemos que se ejecute en el puerto 4000, tambien sirve para guardar cambios en el código gracias a la condición.

if __name__ == '__main__':
    app.run(debug=True, port=4000)
