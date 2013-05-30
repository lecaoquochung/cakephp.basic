#cakephp.basic note
=============

Everything basic about cakephp for developing a web system

##Model
###Query
EX:
/*
* user top sales
*/
$system_month = date('Y-m-00',strtotime('now'));
$sale_flag = Configure::read('SALE_FLAG');
//$sales = $this->User->Sale->find('list',array('fields'=>array('Sale.user_id','Sale.target_sale'),'conditions'=>array('Sale.target_month'=>$system_month)));
$sales = $this->User->Sale->find('list',array('fields'=>array('Sale.user_id','Sale.target_sale'),'conditions'=>array('Sale.target_month'=>$system_month)));

$users = $this->paginate('User');

foreach ($users as $key=>$user) {
	$sales_count = $this->Status->find('count',array('conditions'=>array('Status.flag'=>$sale_flag,'DATE_FORMAT(Status.created,"%Y-%m-00")'=>(isset($target_month)?$target_month:$system_month),'Status.user_id'=>$user['User']['id'])));
	$sale_status = $this->Status->find('all',array('fields'=>'SUM(price)','conditions'=>array('Status.flag'=>$sale_flag,'DATE_FORMAT(Status.created,"%Y-%m-00")'=>(isset($target_month)?$target_month:$system_month),'Status.user_id'=>$user['User']['id'])));
	//$users[$key]['User']['sales_status'] = $sales_count * $price;
	$users[$key]['User']['sales_status'] = $sale_status[0][0]['SUM(price)'];
}

$this->set(compact('users','sales'));
/*
* END user top sales
*/

###DB
backup: # mysqldump -u root -p[root_password] [database_name] > dumpfilename.sql

restore:# mysql -u root -p[root_password] [database_name] < dumpfilename.sql

$query = \DB::select('admin_id')->from('admin');
	$query->where(array('admin_user_name' => $userid, 'admin_password' => $password));
	$data = $query->execute()->as_array();

	->join(............)



##View
$this->request->params
EX: $jobhunter_id = $this->request->params['pass'][0]; // get params from url

$this->Session->read('Auth.User.id')

###Element
<td class="actions" id="<?php echo h($status['Status']['id']); ?>">
<?php echo $this->element('flag', array('status'=>$status)); ?>

###Layout
<?php echo $this->fetch('title').__('System Name'); ?>
<?php $this->assign('title', __('User Login'));?>

##webroot
http -> https
RewriteEngine On
RewriteCond %{HTTP_HOST} ^www\.(.*)
RewriteRule ^.*$ https://%1/$1 [R=301,L]

##Controller
Controller::beforeFilter()
This function is executed before every action in the controller. It’s a handy place to check for an active session or inspect user permissions.

Controller::beforeRender()
Called after controller action logic, but before the view is rendered. This callback is not used often, but may be needed if you are calling render() manually before the end of a given action.

Find:
$this->Status->User->find('list',array('conditions'=>array('User.mark_user'=>1)));

public function admin_login() {
	/*if ($this->request->clientIp() != 'xxx.xxx.xxx.xxx') {
		throw new ForbiddenException();
	}*/
}

$this->Status->saveField('modifield',date('Y-m-d H:i:s'));

##Config
###Bootstrap
Configure::read('ADMIN_STATUS_ADD')

##Bake
/home/hungle/.bash_profile alias