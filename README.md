# flask_api
demostrate how to use flask blueprint

## setup virtual environment

```
    $ pip install virtualenv
    $ virtualenv venv
    ```

	```
    $ source venv/bin/activate
    $ pip install flask
    ```
### why __init__.py
this file make the folder modular (importable)
the files inside the directory can be easily importable as modules

### setup directory structure

    ```
    $ mkdir app
    $ cd app
    $ touch __init__.py
    ```

### Inside app folder make api directory

    ```
    $ mkdir api
    $ cd api
    $ touch __init__.py
    ```

### Inside api folder make two directories (model , view )

#### models
    ```
    $ mkdir models
    $ cd model
    $ touch __init__.py
    ```

#### views
    ```
    $ mkdir views
    $ cd views
    $ touch __init__.py
    ```

#### objective
- make a chat lobby where users can add chat message 

## Code (Models)
On the Model directory make a lobby_model.py file

    ```
    $ touch lobby_model.py
    ```
### Instructions

#### model/lobby_model.py

    ```
    - public variables
        - CHAT_LOBBY = []
    - make a class LobbyModel
    - methods
        - init
            self.lobby = CHAT_LOBBY
        -add_chat_to_lobby(chat_message)
            chat = {
                'id': len(self.lobby) + 1,
                'chat': chat_message,
                'time': datetime.now()
            }
            self.register.append(chat)

        -view_lobby_messages()
            return self.lobby
    ```
## Code (Views)
On the Views directory make a lobby_views.py file

    ```
    $ touch lobby_views.py
    ```
### Instructions

#### model/lobby_views.py

   ###### - import lobby_models into lobby_view
    ```from ..models import lobby_models 
        LOBBY = lobby_models.RegisterModel()
    ```
   ###### - make blueprint route
    ``` lobby_route = Blueprint('lobby', __name__,url_prefix='/api/v1/lobby') ```
   ###### GET method to view_lobby_messages
     ``` lobby_route.route('',methods=['GET']) 
        def get_lobby_messages():
        data = LOBBY.view_lobby_messages()
        return make_response(jsonify({
                "status": 200,
                "data": [{"message": data}]})), 200
     ```
   ###### POST chat_messages to lobby
     ``` lobby_route.route('',methods=['POST']) 
        def post_chat_messages(chat):
         LOBBY.add_chat_to_lobby(chat)
         return make_response(jsonify({
            "status": 201,
            "data": [{"message": "success"}]})), 201
     ```
#### app/__init__.py
 - register blueprint routes
###### make function create_app
    ```
    def create_app(config_class=Config)
        - app = Flask(__name__)
        - from .api.v1.views import lobby_views
        - app.register_blueprint(lobby_views.lobby_route)
    ```

### root folder
     ```
     touch run.py
     ```
#### app/__init__.py
     ```
     from app import create_app
     app = create_app()

    if __name__ == "__main__":
        app.run()
     ```