# woocommerce cart items count shortcode
function cart_items_count_shortcode() {
    if (function_exists('WC')) {
        $cart_count = WC()->cart->get_cart_contents_count();
        return $cart_count;
    }
    return '';
}
add_shortcode('cart_items_count', 'cart_items_count_shortcode');
## Usage >>   [cart_items_count] 

----

# Disable Gutenberg

add_filter( 'use_block_editor_for_post', '__return_false' );
add_filter( 'use_widgets_block_editor', '__return_false' );

----

# شمردن تعداد ورودی‌های گرویتی

function gf_count_user_submissions( $atts ) {
    // دریافت اطلاعات کاربر واردشده
    $current_user = wp_get_current_user();
    if ( ! $current_user->exists() ) {
        return 'لطفاً ابتدا وارد حساب کاربری خود شوید.';
    }

    // دریافت مقادیر شورت‌کد
    $atts = shortcode_atts(
        array(
            'form_id' => '', // آی‌دی فرم خاص (اختیاری)
        ),
        $atts
    );

    // چک کردن اینکه آیا گرویتی فرم فعال است
    if ( ! class_exists( 'GFAPI' ) ) {
        return 'افزونه گرویتی فرمز فعال نیست.';
    }

    // فیلتر کردن ورودی‌ها بر اساس کاربر
    $search_criteria = array(
        'field_filters' => array(
            array(
                'key'   => 'created_by',
                'value' => $current_user->ID,
            ),
        ),
    );

    // دریافت آی‌دی فرم
    $form_id = ! empty( $atts['form_id'] ) ? intval( $atts['form_id'] ) : null;

    // شمارش ورودی‌های فرم
    $entry_count = GFAPI::count_entries( $form_id, $search_criteria );

    // نمایش تعداد
    return "تعداد فرم‌های ثبت‌شده: $entry_count";
}
add_shortcode( 'gf_user_submissions', 'gf_count_user_submissions' );

function gf_count_total_entries( $atts ) {
    // دریافت مقادیر شورت‌کد
    $atts = shortcode_atts(
        array(
            'form_id' => '',
        ),
        $atts
    );

    // چک کردن اینکه آیا گرویتی فرم فعال است
    if ( ! class_exists( 'GFAPI' ) ) {
        return 'افزونه گرویتی فرمز فعال نیست.';
    }

    // دریافت آی‌دی فرم
    if ( empty( $atts['form_id'] ) ) {
        return 'لطفاً آی‌دی فرم را مشخص کنید.';
    }
    $form_id = intval( $atts['form_id'] );

    // شمارش کل ورودی‌های فرم (بدون توجه به کاربر)
    $entry_count = GFAPI::count_entries( $form_id );

    // نمایش تعداد
    return " $entry_count";
}
add_shortcode( 'gf_form_total_entries', 'gf_count_total_entries' );


برای شمارش همه فرم‌های ثبت شده کاربر: [gf_user_submissions]
 برای شمارش فرم‌های یونیک: [gf_user_submissions form_id="3"]
برای شمارش تعداد کل ورودی‌های یک فرم خاص (فارغ از کاربر): [gf_form_total_entries form_id="1"]
