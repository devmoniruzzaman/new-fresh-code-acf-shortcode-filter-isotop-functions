

<!-- -------css here---- -->
* { box-sizing: border-box; }

body {
  font-family: sans-serif;
}

/* ---- button ---- */

.button {
  display: inline-block;
  padding: 10px 18px;
  margin-bottom: 10px;
  background: #EEE;
  border: none;
  border-radius: 7px;
  background-image: linear-gradient( to bottom, hsla(0, 0%, 0%, 0), hsla(0, 0%, 0%, 0.2) );
  color: #222;
  font-family: sans-serif;
  font-size: 16px;
  text-shadow: 0 1px white;
  cursor: pointer;
}

.button:hover {
  background-color: #8CF;
  text-shadow: 0 1px hsla(0, 0%, 100%, 0.5);
  color: #222;
}

.button:active,
.button.is-checked {
  background-color: #28F;
}

.button.is-checked {
  color: white;
  text-shadow: 0 -1px hsla(0, 0%, 0%, 0.8);
}

.button:active {
  box-shadow: inset 0 1px 10px hsla(0, 0%, 0%, 0.8);
}

/* ---- button-group ---- */

.button-group:after {
  content: '';
  display: block;
  clear: both;
}

.button-group .button {
  float: left;
  border-radius: 0;
  margin-left: 0;
  margin-right: 1px;
}

.button-group .button:first-child { border-radius: 0.5em 0 0 0.5em; }
.button-group .button:last-child { border-radius: 0 0.5em 0.5em 0; }

/* ---- isotope ---- */

.grid {
  border: 1px solid #333;
}

/* clear fix */
.grid:after {
  content: '';
  display: block;
  clear: both;
}

/* ---- .element-item ---- */

.element-item {
  position: relative;
  float: left;
  width: 100px;
  height: 100px;
  margin: 5px;
  padding: 10px;
  background: #888;
  color: #262524;
}

.element-item > * {
  margin: 0;
  padding: 0;
}

.element-item .name {
  position: absolute;

  left: 10px;
  top: 60px;
  text-transform: none;
  letter-spacing: 0;
  font-size: 12px;
  font-weight: normal;
}

.element-item .symbol {
  position: absolute;
  left: 10px;
  top: 0px;
  font-size: 42px;
  font-weight: bold;
  color: white;
}

.element-item .number {
  position: absolute;
  right: 8px;
  top: 5px;
}

.element-item .weight {
  position: absolute;
  left: 10px;
  top: 76px;
  font-size: 12px;
}

.element-item.alkali          { background: #F00; background: hsl(   0, 100%, 50%); }
.element-item.alkaline-earth  { background: #F80; background: hsl(  36, 100%, 50%); }
.element-item.lanthanoid      { background: #FF0; background: hsl(  72, 100%, 50%); }
.element-item.actinoid        { background: #0F0; background: hsl( 108, 100%, 50%); }
.element-item.transition      { background: #0F8; background: hsl( 144, 100%, 50%); }
.element-item.post-transition { background: #0FF; background: hsl( 180, 100%, 50%); }
.element-item.metalloid       { background: #08F; background: hsl( 216, 100%, 50%); }
.element-item.diatomic        { background: #00F; background: hsl( 252, 100%, 50%); }
.element-item.halogen         { background: #F0F; background: hsl( 288, 100%, 50%); }
.element-item.noble-gas       { background: #F08; background: hsl( 324, 100%, 50%); }

<!-- -------css here---- -->






<?php

if (!defined('ABSPATH')) die();

add_image_size( 'work-thumbs-size', 400, 230, true );

function divi_child_theme_enqueue_styles() {

    wp_enqueue_style( 'parent-style', get_template_directory_uri() . '/style.css' );

    wp_enqueue_script( 'imagesloaded.pkgd.min-js',  '//unpkg.com/imagesloaded@5/imagesloaded.pkgd.min.js', array ( 'jquery' ));

    wp_enqueue_script( 'isotope-js',  '//npmcdn.com/isotope-layout@3/dist/isotope.pkgd.js', array ( 'jquery' ));
    
    wp_enqueue_script( 'main-js', get_stylesheet_directory_uri() . '/js/main.js', array ( 'jquery' ), 1.1, true);

}

add_action( 'wp_enqueue_scripts', 'divi_child_theme_enqueue_styles' );


//Year

function func_year( $atts ) {

 return date("Y");

}
add_shortcode( 'year','func_year' );

// ------------our works---

function project_shortcode_callback_func_our_works( $atts = array(), $content = '' ) {

ob_start(); ?>
<script type="text/javascript">
    jQuery(document).ready(function($){
// init Isotope
var $grid = $('.grid').isotope({
  itemSelector: '.element-item',
  layoutMode: 'fitRows',
    fitRows:{
    equalheight:true
  }
});


// layout Isotope after each image loads
$grid.imagesLoaded().progress( function() {
  $grid.isotope('layout');

});


// bind filter button click
$('.filters-button-group').on( 'click', 'button', function() {
  var filterValue = $( this ).attr('data-filter');

  $grid.isotope({ filter: filterValue });
});

// change is-checked class on buttons
$('.button-group').each( function( i, buttonGroup ) {
  var $buttonGroup = $( buttonGroup );
  $buttonGroup.on( 'click', 'button', function() {
    $buttonGroup.find('.is-checked').removeClass('is-checked');
    $( this ).addClass('is-checked');
  });
});

});
</script>

<div class="button-group filters-button-group">
<span>Filter by</span>
  <button class="button is-checked" data-filter="*"> All</button>
        <?php

            $terms = get_terms(


                array(
                    'taxonomy'   => 'work-category',
                    'hide_empty' => false,
                )
            );



            // echo "<pre>";
            // var_dump($terms); 
            // echo "</pre>";

            // Check if any term exists
            if ( ! empty( $terms ) && is_array( $terms ) ) {
                // Run a loop and print them all
                foreach ( $terms as $term ) { ?>

                    <button data-filter=".<?php echo $term->slug ?>"><?php echo $term->name; ?></button>

                <?php

                }
            } 

         ?>
</div>


<?php 

// The Query.

$args = array(
'post_type' => 'work',
'posts_per_page' => -1,
'order'     => 'ASC',

);


$the_query = new WP_Query( $args );

// The Loop.
if ( $the_query->have_posts() ) {
    echo '<div class="grid grid-project">';
    while ( $the_query->have_posts() ) {
        $the_query->the_post();
                    $terms = get_the_terms( get_the_ID(), 'work-category' );
            $terms = join(' ', wp_list_pluck( $terms , 'slug') );

        echo '<div class="element-item single-project '.$terms.'">';

            echo "<a href='".get_the_permalink()."'>";

                if(has_post_thumbnail()){
                    echo "<div class='img-area'>";
                        the_post_thumbnail('work-thumbs-size');
                    echo "</div>";
                }
                echo "<h3 class='work-title'>".get_the_title()."</h3>";

            echo "</a>";

            
        echo '</div>';
    }
    
    echo'</div>';

} 
// Restore original Post Data.
?>
<?php
return ob_get_clean();

    
}
add_shortcode( 'our_work', 'project_shortcode_callback_func_our_works' );
