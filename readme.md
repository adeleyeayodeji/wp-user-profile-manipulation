```php

class AdeProfileImage
{
	public function init()
	{
		//pre_get_avatar
		add_filter('pre_get_avatar', [$this, 'pre_get_avatar'], 10, 5);
		//init api route
		add_action('rest_api_init', [$this, 'init_api_route']);
	}

	//init_api_route
	public function init_api_route()
	{
		//register rest route
		register_rest_route('ade/v1', '/profile-image', [
			'methods' => 'POST',
			'callback' => [$this, 'upload_profile_image'],
			'permission_callback' => '__return_true',
		]);
	}

	//upload_profile_image
	public function upload_profile_image(WP_REST_Request $request)
	{
		$image_src = $request->get_param('image_src');
		$user_email = $request->get_param('user_email');

		//check user exist
		$user = get_user_by('email', $user_email);
		if (empty($user)) {
			return new WP_Error('error', 'User does not exist', array('status' => 400));
		}
		//check if image_src is empty
		if (empty($image_src)) {
			return new WP_Error('error', 'Image source is empty', array('status' => 400));
		}
		$user_id  = $user->ID;
		//update usermeta
		update_user_meta($user_id, 'ade_profile_image', $image_src);
		//return success
		return new WP_REST_Response(array('status' => 'success', 'user_id' => $user_id, 'image_src' => $image_src, "message" => "Image updated"), 200);
	}

	//pre_get_avatar
	public function pre_get_avatar($avatar, $id_or_email)
	{
		//check email or id
		if (is_numeric($id_or_email)) {
			$user_id = $id_or_email;
		} else {
			$user = get_user_by('email', $id_or_email);
			$user_id = $user->ID;
		}
		//check usermeta ade_profile_image
		$profile_image = get_user_meta($user_id, 'ade_profile_image', true);
		if (!empty($profile_image)) {
			$avatar = "<img src='$profile_image' class='avatar avatar-96 photo' height='96' width='96' alt='Profile image' />";
		}
		return $avatar;
	}
}

//init
$adeProfileImage = new AdeProfileImage();
$adeProfileImage->init();

```
