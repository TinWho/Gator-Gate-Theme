
/*
 * Name:              Gator-Gate-Theme (Recent Post/Topics Component)
 * Description:       CSS Theme for bbPress.
 * Version:                 0.1.0 Alpha
 * Author:      Tin Who
 * Author URI:  https://tinfoilwho.com
 * License:                 GPL-2.0-or-later
 *
 * AI-generated/AI-assisted code provided AS-IS.
 * User assumes all risk and responsibility for use.
 */



/* ==========================================================================
   Gator-Gate-Theme Recent Post/Topics Component - BBPRESS DARK BLUE THEME
   CSS is included within the PHP
   PART 1: CORE CONTAINER & PAGE LAYOUT

   How To Use; add the shortcode [gatorgate_recent_topics] to a page
   ========================================================================== */


add_shortcode('gatorgate_recent_topics', 'gatorgate_recent_topics');

function gatorgate_recent_topics() {

    if ( ! function_exists('bbp_get_topic_post_type') ) {
        return '<p>bbPress is not active.</p>';
    }

    ob_start();

    $paged = isset($_GET['gg_page']) ? max(1, intval($_GET['gg_page'])) : 1;

    $topics_query = new WP_Query(array(
        'post_type'      => bbp_get_topic_post_type(),
        'posts_per_page' => 10,
        'paged'          => $paged,
        'post_status'    => 'publish',
        'orderby'        => 'date',
        'order'          => 'DESC',
        'ignore_sticky_posts' => true
    ));

    if ( $topics_query->have_posts() ) {

        echo '<div class="bbp-gatorgate-widget-tray">';

        $search_url = function_exists('bbp_get_search_url')
            ? bbp_get_search_url()
            : home_url('/');

        ?>

        <form role="search"
              method="get"
              class="bbp-search-form bbp-tray-embedded-search"
              action="<?php echo esc_url($search_url); ?>">

            <input type="hidden" name="action" value="bbp-search-request">

            <input type="text"
                   name="bbp_search"
                   id="bbp_search_tray"
                   placeholder="Search forums...">

            <input class="button"
                   type="submit"
                   id="bbp_search_submit_tray"
                   value="Search">

        </form>

        <?php

        echo '<span class="bbp-gatorgate-widget-heading">';
        echo ' '; // Change if you want a heading
        echo '</span>';

        echo '<ul class="bbp-gatorgate-widget-list">';

        while ( $topics_query->have_posts() ) :

            $topics_query->the_post();

            $topic_id = get_the_ID();

            $author_id = get_post_field('post_author', $topic_id);

            $author_name = get_the_author_meta(
                'display_name',
                $author_id
            );

            $author_link = function_exists('bbp_get_user_profile_url')
                ? bbp_get_user_profile_url($author_id)
                : get_author_posts_url($author_id);

            // Fetch the parent Forum Details
            $forum_id = function_exists('bbp_get_topic_forum_id') 
                ? bbp_get_topic_forum_id($topic_id) 
                : wp_get_post_parent_id($topic_id);

            $forum_title = get_the_title($forum_id);
            $forum_link  = get_permalink($forum_id);

            // Compute paired multi-unit post age
            $post_timestamp = get_post_time('U', true, $topic_id);
            $current_timestamp = current_time('timestamp', true);
            $diff = abs($current_timestamp - $post_timestamp);

            if ($diff < MINUTE_IN_SECONDS) {
                $topic_age = 'Just now';
            } else {
                $months  = floor($diff / (30 * DAY_IN_SECONDS));
                $rem     = $diff % (30 * DAY_IN_SECONDS);
                $weeks   = floor($rem / (7 * DAY_IN_SECONDS));
                $rem     = $rem % (7 * DAY_IN_SECONDS);
                $days    = floor($rem / DAY_IN_SECONDS);
                $rem     = $rem % DAY_IN_SECONDS;
                $hours   = floor($rem / HOUR_IN_SECONDS);
                $rem     = $rem % HOUR_IN_SECONDS;
                $minutes = floor($rem / MINUTE_IN_SECONDS);

                $time_segments = array();

                if ($months > 0) {
                    $time_segments[] = sprintf(_n('%s month', '%s months', $months, 'bbpress'), $months);
                    if ($weeks > 0) {
                        $time_segments[] = sprintf(_n('%s week', '%s weeks', $weeks, 'bbpress'), $weeks);
                    }
                } elseif ($weeks > 0) {
                    $time_segments[] = sprintf(_n('%s week', '%s weeks', $weeks, 'bbpress'), $weeks);
                    if ($days > 0) {
                        $time_segments[] = sprintf(_n('%s day', '%s days', $days, 'bbpress'), $days);
                    }
                } elseif ($days > 0) {
                    $time_segments[] = sprintf(_n('%s day', '%s days', $days, 'bbpress'), $days);
                    if ($hours > 0) {
                        $time_segments[] = sprintf(_n('%s hour', '%s hours', $hours, 'bbpress'), $hours);
                    }
                } elseif ($hours > 0) {
                    $time_segments[] = sprintf(_n('%s hour', '%s hours', $hours, 'bbpress'), $hours);
                    if ($minutes > 0) {
                        $time_segments[] = sprintf(_n('%s minute', '%s minutes', $minutes, 'bbpress'), $minutes);
                    }
                } else {
                    $time_segments[] = sprintf(_n('%s minute', '%s minutes', $minutes, 'bbpress'), $minutes);
                }

                $topic_age = implode(', ', $time_segments) . ' ago';
            }

            ?>

            <li class="bbp-gatorgate-widget-item">

                <a href="<?php the_permalink(); ?>"
                   class="bbp-gatorgate-widget-title">

                    <?php the_title(); ?>

                </a>

                <span class="bbp-gatorgate-widget-meta">

                    <span class="bbp-gatorgate-meta-left">
                        <span class="bbp-gatorgate-author-prefix">by</span>
                        <a href="<?php echo esc_url($author_link); ?>" class="bbp-gatorgate-author-link">
                            <?php echo esc_html($author_name); ?>
                        </a>
                           
                            <span class="bbp-gatorgate-forum-prefix"></span>
                        <a href="<?php echo esc_url($forum_link); ?>" class="bbp-gatorgate-forum-link">
                            <?php echo esc_html($forum_title); ?>
                        </a>
                    </span>

                    <span class="bbp-gatorgate-date">
                        <?php echo esc_html($topic_age); ?>
                    </span>

                </span>

            </li>

            <?php

        endwhile;

        echo '</ul>';

        if ( $topics_query->max_num_pages > 1 ) {

            $current_url = remove_query_arg('gg_page');

            $paginate_url = add_query_arg(
                'gg_page',
                '%#%',
                $current_url
            );

            echo '<div class="bbp-gatorgate-pagination">';

            echo paginate_links(array(
                'base'      => esc_url_raw($paginate_url),
                'format'    => '',
                'current'   => $paged,
                'total'     => $topics_query->max_num_pages,
                'prev_text' => '&larr; Prev',
                'next_text' => 'Next &rarr;'
            ));

            echo '</div>';

        }

        echo '</div>';

    } else {

        echo '<div class="bbp-gatorgate-widget-tray">';
        echo '<p style="color:white;">No recent forum activity found.</p>';
        echo '</div>';

    }

    wp_reset_postdata();

    return ob_get_clean();

}


/*
* Tin Foil Who
* Project: bbPress GatorGate Recent Topics Widget - Part 2: Inline Styles
*/

add_action('wp_head', 'gatorgate_recent_topics_styles');

function gatorgate_recent_topics_styles() {
?>

<style>

/* Hide default bbPress search */
#bbpress-forums div.bbp-search-form:not(.bbp-tray-embedded-search) {
    display:none !important;
}

/* Customise the page title size and vertical spacing on page 5378 */
.page-id-5378 .entry-title, 
.page-id-5378 .page-title, 
.page-id-5378 .post-title,
.page-id-5378 header h1,
.page-id-5378 main h1.entry-title {
    display: block !important; 
    font-size: 24px !important; 
    padding-top: 38px !important;    
}
    
/* Main container layout */
.bbp-gatorgate-widget-tray {
    background:#130530 !important;
    border:1px solid #331178 !important;
    border-radius:8px !important;
    padding:24px 20px 24px 20px !important; 
    margin-top: -12px !important; 
    margin-bottom:20px !important;
    margin-left: -9px !important;
    margin-right: 9px !important;
    box-sizing:border-box !important;
}
    
/* Embedded Search bar rules */
.bbp-tray-embedded-search {
    display:flex !important;
    gap:6px !important;
    margin-bottom:25px !important;
}

#bbp_search_tray {
    flex:1 !important;
    height: 30px !important;
    padding:2px 10px !important;
    background:#130530 !important;
    color:#fff !important;
    border:1px solid #331178 !important;
    border-radius:4px !important;
}

#bbp_search_tray:focus {
    border-color:#38bdf8 !important;
    outline:none !important;
}

#bbp_search_submit_tray {
    width:75px !important;
    height:30px !important;
    background:#152244 !important;
    color:#fff !important;
    border:1px solid #331178 !important;
    border-radius:4px !important;
    cursor:pointer !important;
    line-height: 1 !important;
    padding-top: 2px !important;     
    padding-bottom: 0px !important;  
    padding-left: 12px !important;
    text-align: center !important;
}

#bbp_search_submit_tray:hover {
    border-color:#38bdf8 !important;
}

/* Heading values */
.bbp-gatorgate-widget-heading {
    display:block !important;
    color:#fff !important;
    font-size:16px !important;
    font-weight:700 !important;
    margin-bottom:12px !important;
}

/* Individual Topic items */
.bbp-gatorgate-widget-list {
    list-style:none !important;
    padding:0 !important;
    margin:0 !important;
}

.bbp-gatorgate-widget-item {
    background:#130530 !important;
    border:1px solid #331178 !important;
    border-radius:6px !important;
    padding:12px 14px !important;
    margin-bottom:10px !important;
    transition:.2s ease !important;
}

.bbp-gatorgate-widget-item:last-child {
    margin-bottom:0 !important;
}

.bbp-gatorgate-widget-item:hover {
    background:#152244 !important;
    border-color:#38bdf8 !important;
}
/* post topice fonr size    */
.bbp-gatorgate-widget-title {
    color:#fff !important;
    font-size:16px !important;
    text-decoration:none !important;
}
/* author and forum font size    */
/* Metadata Flex alignment framework */
.bbp-gatorgate-widget-meta {
    display:flex !important;
    justify-content:space-between !important;
    align-items:center !important;
    margin-top: 10px !important;
    font-size:14px !important;
    color:#8a7fa6 !important;
}

.bbp-gatorgate-meta-left {
    display:inline-block !important;
}

.bbp-gatorgate-author-prefix,
.bbp-gatorgate-author-link {
    color:#8da2fb !important;
}

.bbp-gatorgate-author-link:hover {
    color:#38bdf8 !important;
}

/* Forum origin styles */
.bbp-gatorgate-forum-prefix {
    color:#8da2fb !important;
    margin-left: 2px !important;
}

.bbp-gatorgate-forum-link {
    color:#38bdf8 !important;
    text-decoration:none !important;
}

.bbp-gatorgate-forum-link:hover {
    text-decoration:underline !important;
}
/* day, weeks, months old color and font size    */
/* Dynamic right side layout age */
.bbp-gatorgate-date {
    color:#ffffff !important;
    font-size:16px !important; 
    font-weight:500 !important;
}

/* Bottom pagination layout drawer */
.bbp-gatorgate-pagination {
    display: flex !important;
    justify-content: center !important;
    margin-top: 20px !important;
    padding-top: 10px !important;
}
/* page numbers at the bottom font size    */
.bbp-gatorgate-pagination .page-numbers {
    color: #8da2fb !important;
    padding: 6px 12px !important;
    margin: 0 4px !important;
    border: 1px solid #331178 !important;
    border-radius: 4px !important;
    text-decoration: none !important;
    font-size: 16px !important;
    background: #130530 !important;
}

.bbp-gatorgate-pagination .page-numbers.current {
    background: #152244 !important;
    color: #fff !important;
    border-color: #38bdf8 !important;
}

.bbp-gatorgate-pagination .page-numbers:hover:not(.current) {
    border-color: #38bdf8 !important;
    color: #38bdf8 !important;
}

/* this is the gap between author and forum*/
.bbp-gatorgate-forum-prefix {
    color: #8da2fb !important;
    margin-left: 10px !important; /* Increase this number to widen the gap */
}

    
</style>
<?php
}
