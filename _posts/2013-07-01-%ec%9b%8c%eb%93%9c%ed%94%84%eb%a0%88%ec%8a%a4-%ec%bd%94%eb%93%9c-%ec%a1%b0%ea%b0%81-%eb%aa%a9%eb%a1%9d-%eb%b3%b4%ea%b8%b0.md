---
title: '[워드프레스 코드 조각] 목록 보기'
author: 녹풍(綠風, Windgreen)
layout: post
permalink: /archives/10474
daumview_id:
  - 47229159
categories:
  - WordPress
tags:
  - WordPress Tip
---
워드프레스엔 기본적으로 제공하는 목록 버튼이 없다. 블로그로 출발해서 그런지 모르겠지만, 여튼간에 간단하게 목록 보기를 구현해 보자.

`post`의 경우와 custom post type을 모두 대응할 수 있도록 짜 봤다.

    if( ! function_exists('fu_get_list_url')){
        /**
         * post 형식 글의 single 페이지에 달 목록 버튼의 링크를 리턴한다.
         * 1페이지에 들어가는 글의 개수는 워드프레스 읽기 설정으로 가정하고 계산한다.
         * 자동으로 taxonomy와 term을 가져와서 링크를 만든다.
         * 여러 개의 taxonomy와 여러 개의 term에 속한 글인 경우엔
         * 워드프레스가 돌려준 배열의 첫 번째 놈을 선택해서 링크를 돌려 준다.
         * @param null $taxonomy
         * @param null $term
         * @return string|void
         */
        function fu_get_list_url($taxonomy = NULL, $term_slug = NULL){
            global $post, $table_prefix;
    
            if( ! $taxonomy){
                $taxonomies = get_post_taxonomies();
                $taxonomy = $taxonomies[0];
            }
    
            if( ! $term_slug){
                $terms = wp_get_post_terms($post->ID, $taxonomy);
                $term = $terms[0];
            }else{
                $term = get_term_by('slug', $term_slug, $taxonomy);
            }
    
            if($term){
                $sql = "SELECT SQL_CALC_FOUND_ROWS {$table_prefix}posts.ID
                    FROM {$table_prefix}posts
                    INNER JOIN {$table_prefix}term_relationships ON ( {$table_prefix}posts.ID = {$table_prefix}term_relationships.object_id )
                    WHERE 1 =1
                    AND (
                    {$table_prefix}term_relationships.term_taxonomy_id
                    IN ( $term->term_id )
                    )
                    AND {$table_prefix}posts.post_type = '{$post->post_type}'
                    AND (
                    {$table_prefix}posts.post_status = 'publish'
                    OR {$table_prefix}posts.post_status = 'private'
                    )
                    GROUP BY {$table_prefix}posts.ID
                    ORDER BY {$table_prefix}posts.post_date DESC";
                $result = mysql_query($sql);
                while($row = mysql_fetch_array($result)){
                    $ids[] = $row['ID'];
                }
    
                $current_index = 0;
                foreach ($ids as $index => $ID) {
                    if($ID == $post->ID){
                        $current_index = $index + 1;
                    }
                }
                $curr_page = ceil($current_index / get_option('posts_per_page'));
    
                // term 로드 결과 있으면
                return get_term_link($term) . "/page/" . $curr_page;
            }else{
    
                $sql = "SELECT SQL_CALC_FOUND_ROWS {$table_prefix}posts.ID
                    FROM {$table_prefix}posts
                    WHERE 1 =1
                    AND {$table_prefix}posts.post_type = '{$post->post_type}'
                    AND (
                    {$table_prefix}posts.post_status = 'publish'
                    OR {$table_prefix}posts.post_status = 'private'
                    )
                    ORDER BY {$table_prefix}posts.post_date DESC";
                $result = mysql_query($sql);
                while($row = mysql_fetch_array($result)){
                    $ids[] = $row['ID'];
                }
    
                $current_index = 0;
                foreach ($ids as $index => $ID) {
                    if($ID == $post->ID){
                        $current_index = $index + 1;
                    }
                }
                $curr_page = ceil($current_index / get_option('posts_per_page'));
    
                // 없으면(custom post type의 경우 term이 아예 없을 수 있다.)
                return home_url('/page/' . $curr_page . '/?post_type=' . $post->post_type);
            }
        }
    }