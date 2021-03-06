# PHP SharePoint Lists API#

The PHP SharePoint Lists API is designed to make working with SharePoint List in PHP easier and more straight forward (no need to worry about SOAP).
The current version of the class only includes methods to add, edit, remove and read from existing SharePoint lists. 

All methods will return Array as results by default. SetReturn type can be used to specify that results should be returned as objects.

This code is licensed under the MIT Licence and has only been tested on SharePoint 2007.

### Usage Instructions

Download the WSDL file for the Lists that you want to interact with, this can normally be obtained at:
    sharepoint.url/subsite/_vti_bin/Lists.asmx?WSDL

Include the SharePointAPI in to your project and create a new instance of it.
The script requires a user account with access to the list in order to function.

    $sp = new SharePointAPI('<username>', '<password>', '<path_to_WSDL>');


#### Reading from a List.

To return all items from a list use:

    $sp->read('<list_name>'); 

To return only the first 10 items from a list use:

    $sp->read('<list_name>', 10); 

To return all the items from a list where surname is smith use:

    $sp->read('<list_name>', null, array('surname'=>'smith')); 

To return the first 5 items where the surname is smith and the age is 40

    $sp->read('<list_name>', 5, array('surname'=>'smith','age'=>40)); 
	
To return the first 10 items where the surname is "smith" using a particular view, call: (It appears views can only be referenced by their GUID)

    $sp->read('<list_name>', 10, array('surname'=>'smith','age'=>40),'{0FAKE-GUID001-1001001-10001}'); 
	
To return the first 10 items where the surname is smith, ordered by age use:

    $sp->read('<list_name>', 10, array('surname'=>'smith'), null, array('age' => 'desc')); 
	
By default List item's are returned in the form of an Array. If you would prefer the results to return in object form, use 

	$sp->setReturnType('object'); 
	
Before invoking any read operations.


#### Adding to a list

To add a new item to a list you can use either the method "write" or "insert" (both function identically). Creating a new record in a List with the headings forename, surname, age, phone may look like:

    $sp->write('<list_name>', array('forename'=>'Bob','surname' =>'Smith', 'age'=>40, 'phone'=>'(00000) 000000' ));


#### Editing Rows

To edit a row you need to have its ID. Assuming the above row had the ID 5, we could change Bob's name to James with:

    $sp->update('<list_name>','5', array('forename'=>'James'));


#### Deleting Rows

To remove rows an ID is also required, to remove the record for James with the ID 5 you would use:

    $sp->delete('<list_name>', '5');
	
#### CRUD - Create, Read, Update and Delete
The above actions can also be performed using the CRUD wrapper on a list. This may be useful when you
want to perform multiple actions on the same list. Crud methods do not require a list name to be passed in.

	$list = $sp->CRUD('<list_name>');
	$list->read(10);
	$list->create(array( 'id'=>1, 'name'=>'Fred' ));
	
	
#### List all Lists.
You can get a full listing of all avaiable lists within the connected sharepoint subsite by calling:

	$sp->getLists();
	
#### List metaData.
You can access a lists meta data (Column configurtion for example) by calling

	$sp->readListMeta('My List');
	
By default the method will attempt to strip out non-useful columns from the results. If you'd like the full results to be returned call:

	$sp->readListMeta('My List',false);

	
	
	
	
