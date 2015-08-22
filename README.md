# ci_pheryphp
A simple class that allows you to use the popular phery-php library for unobtrusive ajax in CodeIgniter.


Initialize phery by scanning the target class for response methods.
This eliminates the need for phery::instance->set( ... array of response methods ), for quick setup.

An data-remote method defined on the client side should have a corresponding response method
with the same name prefixed by "ph_". Usage example:

Required Assets:

Please include the phery.js library included in the assets folder "/pheryutils/assets/phery.js" into your main templates header.

View:

   <button data-remote="testmethod" >Call ph_testmethod in target class</button>
    or equivalent
   <?php echo phery::link_to('Call ph_testmethod in target class', 'testmethod', array('tag' => 'button')); ?>

You can initilize the phery utility in your global MY_Controller class or in the Controller class you care to use it in.

MY_Controller:

```php

 var $phery = null;

 public function __construct() {
    // set up some stuff before heading off to phery land:
    $this->load->library("pheryutils");
    $this->phery = PheryResponse::factory();
 }

 public function ph_testmethod($data = NULL) {  // note the function prefix of ph_
   return PheryResponse::factory()->alert('Hello from ph_testmethod!');
 }

 public function ph_save($form_data = array()) {
    //create a phery response factory.
    $response = PheryResponse::factory();
    $firstName = $form_data['FirstName']
    // do your other work.
    //if you're returning a view then set it up
    $view = $this->load->view('myview/sampleview',$datapasstoview, TRUE);
    //and finally lets setup the phery response object we'll be returning to the client.
    $response = $response->jquery('.main')->html($view)->script('filter();');
    //note the use of ->jquery() - yes you can do all kinds of cool jquery stuff.
    //In this case we're setting a div with the class of .main equal to the returned view
    //and then we're calling a javascript function called filter();
 }

```