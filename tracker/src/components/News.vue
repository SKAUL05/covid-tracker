<template>
<div>
    <div class="grid sm:grid-cols-1 md:grid-cols-3 lg:grid-cols-3 xl:grid-cols-3 gap-5" >
        <!--Card 1-->
        <div class="cursor-pointer rounded overflow-hidden shadow-lg" v-for="article in articles" :key="article" @click="newsLink(article.link)">
        <!-- <img class="object-contain md:object-scale-down h-auto" :src="article.image_url" alt=""> -->
        <div class="px-6 py-4">
            <div class="font-bold text-xl mb-2">{{article.title}}</div>
            <p class="text-gray-700 text-base">
                {{ truncate(article.description, 350, "...")}}
            </p>
        </div>
        <div class="px-6 pt-4 pb-2">
            <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">Author: {{checkNull(article.creator)}}</span>
            <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">Published: {{publishedAt(article.pubDate)}}</span>
            <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">Source: {{article.source_id}}</span>
        </div>
        </div>
    
    </div>
</div>
</template>

<script>
import moment from 'moment'

export default {
  name:'News',
  props:['articles'],
  methods: {
        publishedAt(dt) {
            return moment(dt).format('MMMM Do YYYY, h:mm:ss a')
        },
        newsLink(link){
             window.open(link, "_blank");    
        },
        checkNull(creator){
            if (creator){
                return creator[0]
            }else{
                return ""
            }
        },
        truncate(str, max, suffix) {
            return str.length < max ? str : `${str.substr(0, str.substr(0, max - suffix.length).lastIndexOf(' '))}${suffix}`
        } 
    }
}
</script>
