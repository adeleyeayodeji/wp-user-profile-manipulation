```php

function learn_press_pre_get_avatar_callback( $avatar, $id_or_email = '', $size ) {

		if ( ! $profile->is_enable_avatar() ) {
			return $avatar;
		}

		if ( ! $user_id ) {
			return $avatar;
		}

		$avatar = '<img alt="Admin bar avatar" src="' . esc_attr( $profile_picture_src ) . '" class="avatar avatar-' . $img_size . ' photo" height="' . $height . '" width="' . $width . '" />';
		

		return $avatar;
	}
}
add_filter( 'pre_get_avatar', 'learn_press_pre_get_avatar_callback', 1, 5 );

```
