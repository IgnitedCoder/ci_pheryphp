# ci_pheryphp
A simple class that makes it easy for you to use the popular phery-php library for unobtrusive ajax in CodeIgniter.

This is a work in progress. If you find bugs or plan to update this library then please do so and submit a PR.

Happy CI Coding.

Initialize phery by scanning the target class for response methods.
This eliminates the need for phery::instance->set( ... array of response methods ), for quick setup.

An data-remote method defined on the client side should have a corresponding response method
with the same name prefixed by "ph_". Usage example:

Required Assets:

Please include the phery.js library included in the assets folder "/pheryutils/assets/phery.js" into your main templates header.

For a demo on what you can do with phery php see: https://github.com/pheryjs/phery

View:

```php

    <script src="<?php echo base_url('assets/js/jquery.js');?>"></script>
    <script src="<?php echo base_url('assets/js/phery.js');?>"></script>
    <?php echo Phery::link_to('Call a function named ph_test in target class', 'testmethod', array('tag' => 'button')); ?>

```

You can initilize the phery utility in your global MY_Controller class or in the Controller class you care to use it in.

MY_Controller:

```php

 var $phery = null;

 public function __constructor() {
    // set up some stuff before heading off to phery land:
    $this->load->library("pheryutils");
    $this->phery = PheryResponse::factory();
    $this->pheryutils->init($this); //initialize the phery handler.
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
    return $response;
 }

    //Other cools things you can do
    //checkout the phery docs, they are extensive.

    public function ph_dojquerystuff() {

    $response = PheryResponse::factory();

    $response->jquery('#submit_order')->show()
              ->jquery('#door-options')->show()
              ->jquery('#search_fixtures')->show()
              ->jquery('#door-selector')->show()
              ->jquery('#edit_order')->hide()
              ->jquery('#confirm_order')->hide()
              ->jquery('#state')->val('new')
              ->jquery('#confirm')->val('')
              ->jquery('.del')->show()
              ->jquery('#delete_fixture')->show()
              ->script('grid.setOptions({editable:true});grid.render();');

    return $response;

    }

    //And finally forms

    <?php echo Phery::form_for('/url-to-action/or/empty-means-current-url', 'function_name', array('class' => 'form', 'id' => 'form_id', 'submit' => array('disabled' => true, 'all' => true))) ?>
        <input type="text" name="text">
        <input type="password" name="pass">
        <input type="submit" value="Send">
    </form>

```