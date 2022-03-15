
# Sesión 05: Procesamiento Programado

## :dart: Objetivos

- Entender los conceptos basicos del procesamiento programado
- Aprender la correcta sintaxis para implementar procesamiento programado
- Construir un process builder con procesamiento programado en Apex

## ⚙ Caso de uso o escenario

Supongamos un caso comercial cuando un usuario tiene que crear una cantidad de contactos para una cuenta. Los usuarios normalmente tienen que hacer clic en muchos campos para crear un número de contactos para una cuenta. Una forma de automatización esperada por el cliente es agregar un campo "Número de contactos" mientras crea una cuenta.

Con base en este número, el cliente espera crear N numero de registros de contactos básicos automáticamente. Los usuarios luego completarán o actualizarán el campo apropiado en los contactos. Se espera que esto ahorre una cantidad sustancial de tiempo y también mejore la adopción por parte del usuario.

Este ejemplo a continuación explicará cómo crear la clase apex y el generador de procesos para "Insertar 'n' número de contactos según el campo 'Número de contactos' mientras se crea el registro de cuenta". Con estos pasos, puede comprender cómo llamar a Apex desde los flujos del generador de procesos.

1. Cree el campo custom:

•	Crea un campo custom llamado “Numero de contactos” en el object Account (Tipo de dato:  Numerico).

2. Crea una clase apex:
 
/**
 Class Name: createchildRecords
 Purpose: Create N number of contacts based on the field ‘Number of contacts’ while Account is created.  
**/
 
public class CreateChildRecords  {
@InvocableMethod (label=’Create Contacts’) 
public Static void createContact(List<Id> accIds){ 
List<Contact> conListToInsert = new List<Contact>(); //list to collect and insert the contacts 
//Query the accounts 
List<Account> accList = [Select Id, Name, Number_of_Contacts__c from Account where Id =:accIds];
//loop through the accounts and create contacts
for(Account acc : accList)
{
if(acc.Number_of_Contacts__c != null)
{
for(integer i=1;i<=acc.Number_of_Contacts__c;i++)
{
Contact con = new Contact();
con.LastName = ‘Invocable’ + acc.Name +’ ‘+ i;
con.AccountId = acc.Id;
conListToInsert.add(con);
 }  }  }
if(!conListToInsert.isEmpty()) {
insert conListToInsert; }   } }

3. Crea el process builder:

   Crea un process builder que llamaremos, creacion de contactos.
                                                 
![process_builder](https://user-images.githubusercontent.com/523243/158457992-610389c0-8b6f-4b23-864f-00fe16bfc10f.png)
                                                                                             
4. Prueba los resultados
                                                 
•	Navega al objecto Account, crea una nueva cuenta e ingresa 4 en el campo ‘Numero de contactos’.
•	Guarda la cuenta.
•	Query el objecto contactos. 
•	Deben existir 4 contactos creados para la Account con detalles basicos.  
 




