# Crud
/*
CRUD graciassss
*/
#include <iostream>
#include <clocale> 
#include <fstream>
#include <string>
#include <cctype>
#include <cstdlib>
using namespace std;
//Menú.
void menu();
//Es entero.
bool esEntero(string linea);
//Create.
void create(int num_l);
//Read.
void read();
string read_up(); 
//Update | Delete.
void update(string& text_up, string buscar, string rempl);
//Update Actualizar.
void update_act(string text);
int main(){
	//Usaremos la función para que la consola interprete. 
	//las palabras acentuadas:
	setlocale(LC_CTYPE,"Spanish");
	int opc=0, num=0;
	string opc_str;
	bool repite = true;
	string usuario, clave, user, password;//admin
	string buscar_n, rempl_n, buscar_a, rempl_a, text;//update
	string text_copy, text_copy1;//delete
	
	do{
	//Admin: Aquí Ingresamos nuestros datos
	//Para entrar a la base de datos
	usuario="admin";
	clave="admin";
	cout<<"\t==================="<<endl;
	cout<<"\t|Ingrese sus Datos|"<<endl;
	cout<<"\t|  Para Ingresar  |"<<endl;
	cout<<"\t==================="<<endl;
	cout<<"\t Usuario:";
	getline(cin,user);
	cout<<"\t Clave:";
	getline(cin,password);
	cout<<"\t==================="<<endl;
	if((usuario==user) &&(clave==password)){
	do
	{
	
	do {
		//Menú Principal del Programa...
		menu ();
		cout<<"\t";
		getline(cin, opc_str);
		cout<<"\n"<<endl;
		//La usaremos para que el usario.
		//Solo ingrese valores enteros.
		if (esEntero(opc_str)) {
			repite = false;
		} else {
			cout<<"\n\tPor favor, Ingrese lo valores del 1 al 5.\n"<<endl;
		}
	} while (repite);
	opc = atoi(opc_str.c_str());
	switch(opc){
		case 1:
			//Create. Ejecución.
			cout<<"\n\tDigite Cuanto Libro va ingresar al sistema \n"<<endl;
			cout<<"\t  "; 
			cin>>num;
			cout<<"\n\n";
			cout<<"\t=========================\n"<<endl;
			cin.ignore();
			create(num);
		break;
		case 2:
			//Read. Ejecución.
			read();
		break;
		case 3:
			//Update. Ejecución.
			text=read_up();
			cin.ignore();
			
			cout<<"\n\tNombre de Libro: "<<endl;
			
			cout<<"\n\tIngrese el nombre a buscar: "<<endl;
			
			cout<<"\n\t";
			getline(cin,buscar_n);
			
			
			cout<<"\n\tIngrese el nuevo nombre: "<<endl;
			
			cout<<"\n\t";	
			getline(cin,rempl_n);
			cout<<"\n";
			//update
			update(text, buscar_n, rempl_n);
			
			cout<<"\n\tNombre de Autor: "<<endl;
			
			cout<<"\n\tIngrese el nombre a buscar: "<<endl;
			
			cout<<"\n\t";
			getline(cin,buscar_a);		
			
			cout<<"\n\tIngrese el nuevo nombre: "<<endl;
			
			cout<<"\n\t";
			getline(cin,rempl_a);
			
			update(text, buscar_a, rempl_a);	
			
			cout<<"\n\tNombres Actualizados...\n\n"<<endl;		
			//update_atc
			update_act(text);
		break;
		case 4:
			//Delete. Ejecución.
			text = read_up();
			cin.ignore();
			cout<<"\n\tNombre del Libro a Borrar: \n"<<endl;
			cout<<"\n\t";
			getline(cin,buscar_n);
			text_copy+= " Nombre de Libro: "+buscar_n+"\n\n";
			cout<<"\n\tNombre de Autor a Borrar: \n"<<endl;
			cout<<"\n\t";
			getline(cin,buscar_a);
			text_copy1+= " Autor: "+buscar_a+"\n\n";
			rempl_a=" ";
			update(text,text_copy,rempl_a);
			update(text,text_copy1,rempl_a);
			cout<<"\n\tLibro Borrado...\n"<<endl;
			update_act(text);
		break;
		
		case 5:
			cout<<"\n\tEjecución Terminada..."<<endl;
		break;
		
		default:
			cout<<"\n\tPor favor, Ingrese lo valores del 1 al 5.\n"<<endl;
		break;	
	}
	}while(opc!=5);
	}else{
		cout<<"\n\tContraseña Incorrecta\n"<<endl;
	}
	}while(opc!=5);	
	getchar();
	return 0;
}
//Menú Definición:
void menu(){
	//Aquí vamos a Desarrollar la Función de menu Principal...
	string linea, text;
	ifstream archivo("base_de_datos/menu.txt");
	if(archivo.fail()){
			cout<<"\n\tArchivo no encontrado."<<endl;
			exit(1);
		}
	//Lectura de archivos
	while (getline(archivo, linea)){
		text+="\t"+linea+"\n";
	}
	archivo.close();
	cout<<text<<endl;
}
//Es entero Definición:
bool esEntero(string linea){
	bool esEntero = true;
	int longitud = linea.size();
	
	if (longitud == 0) {
		esEntero = false;
	} else if (longitud == 1 && !isdigit(linea[0])) {
		esEntero = false;
	} else {
		int indice = 0;
		if (linea[0] == '+' || linea[0] == '-') {
			indice = 1;
		} else {
			indice = 0;
		}
		
		while (indice < longitud) {
			if (!isdigit(linea[indice])) {
				esEntero = false;
				break;
			}
			indice++;
		}
	}
	
	
	return esEntero;	
}
//Create definición:
void create(int num_l){
	// Creación de struct, var, y archivo.... 
	struct libro
	{
		string nombre_l;	
		string autor_l;
	}libro[50];
	//Aquí creamos el archivo desde cero.
	string libros[num_l],text;
	ofstream archivo("base_de_datos/read.txt");
	//Nos Avisará si el archivo no se encuentra.
	if(archivo.fail()){
		cout<<"\n\tArchivo no encontrado."<<endl;
		exit(1);
	}
	//Create
	//Aquí el usuario ingresara los datos de lo libros.
	//Qué él o ella quieran guardar.
	for(int i=0;i<num_l;i++){
	cout<<"\t"<<i+1<<".- Digite el nombre del libro: \n"<<endl; 
	cout<<"\t  "; 
	getline(cin,libro[i].nombre_l); 
	cout<<"\n";
	cout<<"\t"<<i+1<<".- Digite el nombre del autor: \n"<<endl; 
	cout<<"\t  "; 
	getline(cin,libro[i].autor_l); 
	cout<<"\n";
	//Aquí vamos a meter lo datos, Ingresados por el usuario.
	//Para luego ingresarlos en un array unidimensional.
	libros[i]=" Nombre de Libro: "+libro[i].nombre_l+"\n\n"+" Autor: "+libro[i].autor_l+"\n\n ";
	//En esta variable guardaremos los datos. 
	//para luego guardarlos en el archivo.
	text += libros[i];
	}
	//Proceso de guardado de datos...
	archivo<<"Lista de Libros disponibles en la Libreria"<<endl;
	archivo<<"\n=========================================="<<endl;
	archivo<<text<<endl;
	cout<<"\tProceso completado... \n"<<endl;
	archivo.close();
}

//Read Definición:
void read(){
	string linea, text;
	//Aqui buscamos el archivo.
	//Lo abrimos y leemos en consola los datos. 
	ifstream archivo("base_de_datos/read.txt");
	if(archivo.fail()){
			cout<<"\n\tArchivo no encontrado."<<endl;
			exit(1);
		}
	//Lectura de archivos
	while (getline(archivo, linea)){
		text+=linea+"\n";
		cout<<"\t"<<linea<<endl;	
	}
	archivo.close();
}

//Read-update:
string read_up(){
	string text;
	string linea;
	ifstream archivo("base_de_datos/read.txt");
	if(archivo.fail()){
			cout<<"\n\tArchivo no encontrado."<<endl;
			exit(1);
		}
	//Lectura de archivos
	while (getline(archivo, linea)){
		text+=linea+"\n";
		cout<<"\t"<<linea<<endl;
	}
	archivo.close();
	return text;
}

//Update | Delete Defición:
//Esto Busca y remplaza lo nombres indicados en el switch(opc 3)...
void update(string& text_up, string buscar, string rempl){
		
	int pos = text_up.find(buscar);
	while(pos!=-1){
		text_up.replace(pos, buscar.size(), rempl);
		pos = text_up.find(buscar, pos + rempl.size());
	}	
}
//Update
//Aqui terminaresmos de actualizar el documento .txt...
void update_act(string text){
	//A continuación lo que haremos es.
	//Guardar, lo remplazado en nuestro archivo..
	// Declaro el osfstream aquí, porque, sino.
	// Le hacer Reser al archivo.
	ofstream archivo("base_de_datos/read.txt");
	if(archivo.fail()){
			cout<<"\n\tArchivo no encontrado."<<endl;
			exit(1);
		}
	archivo<<text<<endl;
	archivo.close();
}
