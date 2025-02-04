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
