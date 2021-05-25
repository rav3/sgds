<template>
  <div>
    <p v-if="hide">Loading...</p>
    <div v-if="!hide">
      <div class="field margin--bottom--lg">
        <label class="label" for="input-1">Text Input Label</label>
        <div class="control">
          <input class="input" type="text" v-model="search" placeholder="search for your question here">
        </div>
      </div>
      <div v-for="faq in filteredFaqs" class="sgds-card">
        <div class="sgds-card-header">
          <p class="sgds-card-header-title margin--bottom--none">{{faq.title}}</p>
          <div class="sgds-tag is-primary is-light">{{faq.tag}}</div>
        </div>
        <div class="sgds-card-content">
          {{faq.desc}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data () {
      return {
          search: '',
          hide: true,
          faqs: []
      }
  },
  mounted () {
    axios.get('/contents/faqs/faqs.json')
    .then(res => {
      this.faqs = res.data
      this.hide = false
    })
    .catch(err => {
      console.log(err.message)
    })

  },
  computed:{
    filteredFaqs: function(){
      return this.faqs.filter(faq => {
        return faq.title.toLowerCase().match(this.search.toLowerCase()) || faq.desc.toLowerCase().match(this.search.toLowerCase()) || faq.tag.toLowerCase().match(this.search.toLowerCase()) 
      })
    }
  }
}
</script>
<style>
.sgds-card + .sgds-card{ margin-top: 1rem;}
.input{border-radius: 5px;}
p.sgds-card-header-title{padding: 0;}
.sgds-card-header{
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem 1rem;
}

</style>