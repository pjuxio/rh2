<template>
  <div class="main-bg">
    <div class="mainbar">
      <h1>Welcome to the Resource Hub!</h1>
      <div class="view-toggle">
        <button @click="toggleViewMode">Toggle View ({{ viewMode }})</button>
      </div>
      </div>
    <div class="items-container" :class="{ 'list-view': viewMode === 'list', 'gallery-view': viewMode === 'gallery' }" ref="itemsContainer">
      <div v-for="item in paginatedItems" :key="item.id" class="item">
        <div v-for="cat in item.Categories ? item.Categories.split('|') : []" :key="cat" :class="['category', categoryClass(cat)]">
          <span v-html="cat"></span>
        </div>
        <img v-if="viewMode === 'gallery'" :src="genericImage" class="resourceImg" alt="Example">
        <h2 class="rh-title"><a :href="item.Link" target="_blank">{{ item.Title }}</a></h2>
        <div class="item-tags">
          <span class="label">Tags:</span>
          <div v-for="tag in item.Tags ? item.Tags.split('|') : []" :key="tag" :class="['tag', tagClass(tag)]">
            <span v-html="tag"></span>
          </div>
        </div>
      </div>
    </div>
    <div class="pagination">
      <button @click="changePage(currentPage - 1)" :disabled="currentPage <= 1">Previous</button>
      <span>Page {{ currentPage }}</span>
      <button @click="changePage(currentPage + 1)" :disabled="currentPage * itemsPerPage >= totalItems">Next</button>
    </div>
  </div>
</template>

<style>

body {
  background-color: #f2f2f2;
}

.mainbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;

}

.list-view .item {
  flex: 0 1 100%; /* Each item takes full width in list view */
  /* Adjust other styling as needed for list view */
}



  .rh-title a {
    margin-top: 0;
    text-decoration: none;
    color: #401338;
  } 
  
  .rh-title a:hover {
    margin-top: 0;
    text-decoration: none;
    color: #10402f;
  } 
  .items-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around; /* This will distribute space around items */
    margin-top: 20px;
  }
  
  .item {
    flex: 0 1 calc(33.333% - 90px); /* On large screens, each item takes up roughly 1/3 of the container width minus some margin */
    margin-bottom: 30px; /* Adds space between rows */
    padding: 20px;
    font-size: .9em;
    border: 1px solid #e6e6e6;
    background: #ffffff;
  }

  /* On medium screens, make items take half width */
  @media (min-width: 768px) and (max-width: 1024px) {
    .item {
      flex: 0 1 calc(50% - 80px); /* Each item takes up roughly half of the container width minus some margin */
      padding: 20px;
    }
  }
  
  .item h2 {
    margin-top: 10px;
  }

  .resourceImg {
    width: 100%;
    height: auto;
  }
  
/* .category-analysis-tool { background-color: #546E7A; color: #ffffff}
.category-art { background-color: #F44336; }
.category-blog { background-color: #E91E63; }
.category-book { background-color: #9C27B0; }
.category-climate-action-plan { background-color: #673AB7; }
.category-clinics-trainings { background-color: #3F51B5; }
.category-data { background-color: #2196F3; color: #ffffff}
.category-documentary { background-color: #03A9F4; }
.category-fellowship { background-color: #00BCD4; }
.category-funded-sabbatical { background-color: #009688; }
.category-funding-opportunity { background-color: #4CAF50; }
.category-guide { background-color: #8BC34A; }
.category-organization { background-color: #CDDC39; }
.category-other { background-color: #FFEB3B; }
.category-other-resource-hubs { background-color: #FFC107; }
.category-podcast { background-color: #FF9800; }
.category-publication { background-color: #FF5722; }
.category-scholarship { background-color: #795548; }
.category-toolkit { background-color: #9E9E9E; }
.category-videos-webinars { background-color: #607D8B; }
.category-zine { background-color: #BDBDBD; }
.category-default { background-color: #D5DBDB; } /* Default class for any category not explicitly defined */
/* Add the rest of your category classes here, following the same pattern. */




  .category {
    display: inline-block;
    padding: 0px 0px;
    margin: 0px 0px 10px 0px;
    border-radius: 4px;
    text-transform: uppercase;
    font-size: 85%;
  }

  .tag {
  display: inline-block;
  padding: 6px 12px;
  margin: 4px;
  border-radius: 4px;
  color: #222222;
}

.tag-air-quality { background-color: #E57373; }
.tag-art { background-color: #F06292; }
.tag-climate-adaptation-resilience { background-color: #BA68C8;color: #FFFFFF; }
.tag-coaching { background-color: #9575CD; color: #ffffff;}
.tag-community-safety { background-color: #7986CB; }
.tag-conservation-biodiversity { background-color: #64B5F6; }
.tag-culture { background-color: #4FC3F7; }
.tag-dei-anti-oppression { background-color: #4DD0E1; }
.tag-economic-justice { background-color: #4DB6AC; }
.tag-emergency-management { background-color: #81C784; }
.tag-energy-justice { background-color: #AED581; }
.tag-environmental-justice { background-color: #DCE775; }
.tag-extreme-heat { background-color: #FFF176; }
.tag-farming-food-justice { background-color: #FFD54F; }
.tag-fossil-fuels { background-color: #FFB74D; }
.tag-fundraising { background-color: #FF8A65; }
.tag-gender-justice { background-color: #A1887F; color: #ffffff;}
.tag-green-space { background-color: #E0E0E0; }
.tag-health-healing { background-color: #90A4AE; color: #ffffff;}
.tag-history { background-color: #81D4FA; }
.tag-housing-tenant-justice { background-color: #80CBC4; }
.tag-housing-justice-tenant-support { background-color: #A5D6A7; }
.tag-indigenous-sovereignty { background-color: #C5E1A5; }
.tag-just-recovery { background-color: #FFCC80; }
.tag-just-transition-framework { background-color: #FFAB91; }
.tag-land-justice { background-color: #BCAAA4;color: #ffffff; }
.tag-legal { background-color: #B0BEC5; }
.tag-organizing { background-color: #B39DDB; }
.tag-policy { background-color: #9FA8DA; color: #ffffff;}
.tag-pollution { background-color: #90CAF9; }
.tag-popular-education { background-color: #81C784; }
.tag-public-health { background-color: #AED581; }
.tag-rest { background-color: #DCE775; }
.tag-sea-level-rise-flooding { background-color: #FFF176; }
.tag-stories-justice-in-action { background-color: #FFD54F; }
.tag-sustainable-buildings { background-color: #FFB74D; }
.tag-technology-justice { background-color: #FF8A65; }
.tag-transportation { background-color: #A1887F;color: #ffffff; }
.tag-videos-webinars { background-color: #E0E0E0; }
.tag-water-equity { background-color: #90A4AE; color: #ffffff;}
.tag-youth { background-color: #81D4FA; }
.tag-default { background-color: #d9d9d9;font-color:#222222;} /* Default class for any category not explicitly defined */

  
  .label {
  display: inline-block; /* Ensures the label respects the width setting */
  width: 35px;
  text-align: left; /* Right-aligns the text, making it look tidy */
  margin-bottom: 10px;
  font-size: 100%;
  font-weight: bold;
}

  
  /* For small screens, make items take full width */
  @media (max-width: 768px) {
    .item {
      flex: 0 1 100%; /* Each item takes full width of the container */
    }
  }

  /* For medium screens, make items take half width */
  @media (min-width: 768px) and (max-width: 1024px) {
    .item {
      flex: 0 1 calc(50% - 80px); /* Each item takes up roughly half of the container width minus some margin */
    }
  }

  .pagination {
    display: flex;
    justify-content: center;
    margin-top: 20px;
  }

  .pagination button {
    margin: 0 10px;
  }

  .view-toggle {
    display: inline-flex;
    margin-bottom: 20px;
  }

  .view-toggle button {
    padding: 10px 20px;
    font-size: 1em;
    font-weight: bold;
    cursor: pointer;
    background-color: #401338;
    color: #ffffff;
    border-radius: 80px;
    border: none;
  }

  .view-toggle button:hover {
    background-color: #D9E026;
    color: #401338;

  }
  
  </style>

<script>
import genericImage from '/public/previews/generic-grey.png';
import axios from 'axios';

export default {
  name: 'FetchDataWithPagination',
  data() {
    return {
      viewMode: 'gallery', // Default to gallery view
      genericImage,
      items: [],
      isLoading: false,
      currentPage: 1,
      itemsPerPage: 15,
      totalItems: 0,
    };
  },
  computed: {
    paginatedItems() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.items.slice(start, end);
    },
  },
  methods: {
    fetchData() {
      this.isLoading = true;
      axios
        .get(`http://localhost:3001/data?page=${this.currentPage}&limit=${this.itemsPerPage}`)
        .then(response => {
          this.items = response.data.data;
          this.totalItems = response.data.total;
          this.isLoading = false;
        })
        .catch(error => {
          console.error("There was an error fetching the data:", error);
          this.isLoading = false;
        });
    },
    changePage(page) {
      this.currentPage = page;
      this.fetchData();
      this.$nextTick(() => {
        const container = this.$refs.itemsContainer;
        if (container) {
          container.scrollIntoView({ behavior: 'smooth' });
        }
      });
    },
    categoryClass(cat) {
      const categoryClasses = {
        'Analysis Tool': 'category-analysis-tool',
        'Art': 'category-art',
        'Blog': 'category-blog',
        'Book': 'category-book',
        'Climate Action Plan': 'category-climate-action-plan',
        'Clinics & Trainings': 'category-clinics-trainings',
        'Data': 'category-data',
        'Documentary': 'category-documentary',
        'Fellowship': 'category-fellowship',
        'Funded Sabbatical': 'category-funded-sabbatical',
        'Funding Opportunity': 'category-funding-opportunity',
        'Guide': 'category-guide',
        'Organization': 'category-organization',
        'Other': 'category-other',
        'Other Resource Hubs': 'category-other-resource-hubs',
        'Podcast': 'category-podcast',
        'Publication': 'category-publication',
        'Scholarship': 'category-scholarship',
        'Toolkit': 'category-toolkit',
        'Videos & Webinars': 'category-videos-webinars',
        'Zine': 'category-zine'
      };
      return categoryClasses[cat] || 'category-default';
    },
    tagClass(tag) {
      const tagClasses = {
        'Air Quality': 'tag-air-quality',
        'Art': 'tag-art',
        'Climate Adaptation &amp; Resilience': 'tag-climate-adaptation-resilience',
        'Coaching': 'tag-coaching',
        'Community Safety': 'tag-community-safety',
        'Conservation &amp; Biodiversity': 'tag-conservation-biodiversity',
        'Culture': 'tag-culture',
        'DEI & Anti-Oppression': 'tag-dei-anti-oppression',
        'Economic Justice': 'tag-economic-justice',
        'Emergency Management': 'tag-emergency-management',
        'Energy Justice': 'tag-energy-justice',
        'Environmental Justice': 'tag-environmental-justice',
        'Extreme Heat': 'tag-extreme-heat',
        'Farming & Food Justice': 'tag-farming-food-justice',
        'Fossil Fuels': 'tag-fossil-fuels',
        'Fundraising': 'tag-fundraising',
        'Gender Justice': 'tag-gender-justice',
        'Green Space': 'tag-green-space',
        'Health &amp; Healing': 'tag-health-healing',
        'History': 'tag-history',
        'Housing &amp; Tenant Justice': 'tag-housing-tenant-justice',
        'Housing Justice & Tenant Support': 'tag-housing-justice-tenant-support',
        'Indigenous Sovereignty': 'tag-indigenous-sovereignty',
        'Just Recovery': 'tag-just-recovery',
        'Just Transition Framework': 'tag-just-transition-framework',
        'Land Justice': 'tag-land-justice',
        'Legal': 'tag-legal',
        'Organizing': 'tag-organizing',
        'Policy': 'tag-policy',
        'Pollution': 'tag-pollution',
        'Popular Education': 'tag-popular-education',
        'Public Health': 'tag-public-health',
        'Rest': 'tag-rest',
        'Sea Level Rise &amp; Flooding': 'tag-sea-level-rise-flooding',
        'Stories/Justice in Action': 'tag-stories-justice-in-action',
        'Sustainable Buildings': 'tag-sustainable-buildings',
        'Technology Justice': 'tag-technology-justice',
        'Transportation': 'tag-transportation',
        'Videos &amp; Webinars': 'tag-videos-webinars',
        'Water Equity': 'tag-water-equity',
        'Youth': 'tag-youth'
      };
      // Normalize the tag to handle '&', spaces, and other characters for CSS class naming
      return tagClasses[tag] || 'tag-default';
    },
    toggleViewMode() {
      this.viewMode = this.viewMode === 'gallery' ? 'list' : 'gallery';
    },
  },
  mounted() {
    this.fetchData();
  },
};
</script>
