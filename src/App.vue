<template>
  <div class="wrapper">
    <h1 :style="{textAlign: 'center'}">URL builder</h1>
    <div id="url-builder">
      <form :style="{width: '72%', margin: '20px auto 50px'}">
        <div class="form-group">
          <label class="label" for="inputTags">URL Base</label>
          <input
            id="inputTags"
            v-model="baseUrl"
            type="text"
            class="form-control"
            aria-describedby="tagsHelp"
            placeholder="https://youtube.com?"
          />
        </div>
        <div class="form-group">
          <label class="label">Source</label>
          <select class="form-control" v-model="tmpSource">
            <option disabled value="">Please select one</option>
            <option v-for="option in source" :value="option.name" :key="option.name">{{ option.name }}</option>
          </select>
        </div>
        <div class="form-group">
          <label class="label">Campaign</label>
          <select v-model="tmpCampaign" class="form-control">
            <option disabled value="">Please select one</option>
            <option v-for="option in campaign" :value="option.name" :key="option.name">{{ option.name }}</option>
          </select>
        </div>
        <div class="form-group">
          <label class="label">Medium</label>
          <select v-model="tmpMedium" class="form-control">
            <option disabled value="">Please select one</option>
            <option v-for="option in medium" :value="option.name" :key="option.name">{{ option.name }}</option>
          </select>
        </div>
        <div class="form-group">
          <label for="term" class="label">Term</label>
          <input
            id="term"
            v-model.trim="term"
            type="text"
            class="form-control"
            aria-describedby="tagsHelp"
          />
        </div>
        <div class="form-group">
          <label for="content" class="label">Content</label>
          <input
            id="content"
            v-model.trim="content"
            type="text"
            class="form-control"
            aria-describedby="tagsHelp"
          />
        </div>
      </form>
      <div class="block_builturl">
        <b>Your built url:</b><br />
        <b
          ><a :href="builtUrl" class="builtUrl">{{ builtUrl }}</a></b
        >
      </div>
    </div>
  </div>
</template>
<script>

export default {
  data() {
    return {
      baseUrl: "",
      builtUrl: "",
      tagSeparator: "|",
      source: source,
      campaign: campaign,
      medium: medium,
      term: "",
      content: "",

      tmpSource: '',
      tmpCampaign: '',
      tmpMedium: ''
    };
  },
  watch: {
    baseUrl() {
      this.buildUrl();
      if(this.baseUrl === ''){
        this.builtUrl = '';
        this.tmpSource = '';
        this.tmpCampaign = '';
        this.tmpMedium = '';
        this.term = '';
        this.content = '';
      }
      
    },
    tmpSource() {
      this.buildUrl();
    },
    tmpCampaign() {
      this.buildUrl();
    },
    tmpMedium() {
      this.buildUrl();
    },
    term() {
      this.buildUrl();
    },
    content() {
      this.buildUrl();
    }
  },
  mounted() {
    this.builtUrl = this.baseUrl;
  },
  methods: {
    buildUrl() {
      let termForUrl =
        this.term.length > 0
          ? encodeURI(
              this.term
                .split(",")
                .map(item => {
                  return item.trim();
                })
                .join(this.tagSeparator)
            )
          : "";
      
      let sourceForUrl =
        this.tmpSource.length > 0
          ? encodeURI(
              this.tmpSource
                .split(",")
                .map(item => {
                  return item.trim();
                })
                .join(this.tagSeparator)
            )
          : "";
      

      let campaignForUrl =
        this.tmpCampaign.length > 0
          ? encodeURI(
            this.tmpCampaign
              .split(",")
              .map(item => {
                return item.trim();
              })
              .join(this.tagSeparator)
            )
          : "";

      let mediumForUrl =
        this.tmpMedium.length > 0
          ? encodeURI(
              this.tmpMedium
                .split(",")
                .map(item => {
                  return item.trim();
                })
                .join(this.tagSeparator)
            )
          : "";

      let contentForUrl =
        this.content.length > 0
          ? encodeURI(
              this.content
                .split(",")
                .map(item => {
                  return item.trim();
                })
                .join(this.tagSeparator)
            )
          : "";

      const encodeUrl = [];

      if (this.baseUrl.indexOf("?") > -1) {
        let source;
        let content;
        let campaign;
        let medium;
        let term;
        let a = this.baseUrl.split("?");
        let b = a[1].split("&");
        for(let i = 0; i < b.length; i++ ) {
            let split = b[i].split('=');
            switch (split[0]) {
              case `utm_source`:
                source =`utm_source=${split[1]}`;
                break;
              case `utm_campaign`:
                campaign = `utm_campaign=${split[1]}`
                break
              case `utm_medium`:
                medium = `utm_medium=${split[1]}`
                break
              case `utm_term`:
                term = `utm_term=${split[1]}`
                break
              case `utm_content`:
                content = `utm_content=${split[1]}`
                break
              default:
                break;
            }
        }
        source = sourceForUrl.length > 0 ? `utm_source=${sourceForUrl}` : source;
        if(source) {
          encodeUrl.push(source);
        }
        campaign = campaignForUrl.length > 0 ? `utm_campaign=${campaignForUrl}` : campaign;
        if(campaign) {
          encodeUrl.push(campaign);
        }
        medium = mediumForUrl.length > 0 ? `utm_medium=${mediumForUrl}` : medium;
        if(medium) {
          encodeUrl.push(medium);
        }
        term = termForUrl.length > 0 ? `utm_term=${termForUrl}` : term;
        if(term) {
          encodeUrl.push(term);
        }
        content = contentForUrl.length > 0 ? `utm_content=${contentForUrl}` : content;
        if(content) {
          encodeUrl.push(content);
        }
        this.builtUrl = `${a[0]}?${encodeUrl.join("&")}`;
       
      }
    }
  }
};
</script>

<style src="./style.css"></style>