# Parcial1-SO

**Universidad Icesi**  
**Curso:** Sistemas Operativos  
**Estudiante:** Melisa Garcia Peña.
**Codigo antiguo:** 13103005.
**Tema:** Comandos de Linux, Servicios web con Python  

Consigne los pasos necesarios para la ejecución y prueba de su solución. Tenga en cuenta incluir la creación del ambiente, activación, apertura de puertos, reinicio de servicios, entre otros .

Primero creo el archivo filesystem_user, accedo a éste y despues creo los entornos virtuales.
en root: cd /home/filesystem_user
$ cd ~/
$ mkdir fylesystem
$ cd filesystem
$ virtualenv flask_filesystem

Despues de haber creado el entorno,hago la activación del ambiente virtual 

$ . flask_filesystem/bin/activate

Después de haber activado el ambiente activo, instalo la libreria Flask

$ pip install Flask


Realice la implementación del servicio web para la URI /files .

Creo los archivos de python uno para los  comandos  y el otro para los metodos “GET”,”POST”,”PUT” y “DELETE”  

$ vi filesystem_01.py

from flask import Flask, abort, request
import json

from filesystem_01_commands import get_all_users, add_user, remove_user
app = Flask(__name__)

api_url = '/v1.0'

@app.route(api_url+'/files',methods=['POST'])
def create_file():
  content = request.get_json(silent=True)
  filename = content['filename']
  content = content['content']
  if not filename or not content:
    return "empty filename or content", 400
  if filename in get_all_file():
    return "file already exist", 400
  if add_file(filename,content):
    return "file created", 201
  else:
    return "error while creating user", 400

@app.route(api_url+'/files',methods=['GET'])
def read_file():
  list = {}
  list["files"] = get_all_file()
  return json.dumps(list), 200

@app.route(api_url+'/files',methods=['PUT'])
def update_file():
  abort(404)

@app.route(api_url+'/files',methods=['DELETE'])
def delete_file():
   error = False
  for filename in get_all_file():
    if not remove_file(filename):
        error = True

  if error:
    return 'some files were not deleted', 400
  else:
    return 'all files were deleted', 200

if __name__ == "__main__":
  app.run(host='0.0.0.0',port=8088,debug='True')


$ vi filesystem_01_commands.py

from flask import Popen, PIPE

def get_all_file():
  grep_process = Popen(["grep","/home/filesystem_user"], stdout=PIPE, stderr=PIPE)
  user_list = Popen(["ls"], stdin=grep_process.stdout, stdout=PIPE, stderr=PIPE).communicate()[0].split('\n')
  return filter(None,file_list)

def add_file(filename,content):
  add_process = Popen(["echo"," ' ",content," ' ",">>","/files",filename], stdout=PIPE, stderr=PIPE)
  add_process.wait()
  return True if username in get_all_file() else False

def remove_file(filename):
  vip = ["carta", "listado", "tareas", "recordatorio"]
  if filename in vip:
    return True
  else:
    remove_process = Popen(["rm",filename], stdout=PIPE, stderr=PIPE)
    remove_process.wait()
    return False if username in get_all_file() else True


Realice la implementación del servicio web para la URI /files/recently_created.

Ahora, se implementan los anteriores metodos, pero para los archivos que se crearon recientemente

from flask import Flask, abort, request
import json

from filesystem_01_commands import get_all_file

app = Flask(__name__)
api_url = '/v1.0'

@app.route(api_url+'/files/recently_created',methods=['POST'])
def create_file():

    return "HTTP 404 NOT FOUND", 404


@app.route(api_url+'/files/recently_created',methods=['GET'])
def read_file():

 list = {}
  list["files"] = get_all_file()

  return json.dumps(list), 200

@app.route(api_url+'/files',methods=['PUT'])
def update_file():
  return "HTTP 404 not found", 404

@app.route(api_url+'/files/recently_created',methods=['DELETE'])
def delete_file():

    return 'HTTP 404 NOT FOUND', 404


if __name__ == "__main__":
  app.run(host='0.0.0.0',port=8088,debug='True')


Comandos

from subprocess import Popen, PIPE

def get_all_file():
  grep_process = Popen(["grep","home/filesystem_user"], stdout=PIPE, stderr=PIPE)
  file_list = Popen(["ls -ltrp | grep -v / | tail -1"], stdin=grep_process.stdout, stdout=PIPE, stderr=PIPE).communicate()[0].split('\n')
  return filter(None,file_list)

La url para la prubas son:


para los archivos:
192.168.130.153:8088/v1.0/files

para los archivos reciente:
192.168.130.153:8088/v1.0/files/recently_created
