<template>
  <div>
  <div class="main-bg">
    <div class="mainbar">
      <h1>Search Results</h1>  
    </div>
    <div class="toolbar">
      <div class="search-field">
       <label for="cat-dropdown">Text Filter:</label><input class="search-field" type="text" v-model="filterText" placeholder="Enter keywords..." />
      </div>
      
      <div class="cat-dropdown">
        <label for="cat-dropdown">Resource Type:</label>
        <select v-model="filterCategoryType" id="cat-dropdown">
        <option value="">All Categories</option>
        <option value="Analysis Tool">Analysis Tool</option>
        <option value="Art">Art</option>
        <option value="Blog">Blog</option>
        <option value="Book">Book</option>
        <option value="Climate Action Plan">Climate Action Plan</option>
        <option value="Clinics and Trainings">Clinics and Trainings</option>
        <option value="Data">Data</option>
        <option value="Documentary">Documentary</option>
        <option value="Fellowship">Fellowship</option>
        <option value="Funded Sabbatical">Funded Sabbatical</option>
        <option value="Funding Opportunity">Funding Opportunity</option>
        <option value="Guide">Guide</option>
        <option value="Organization">Organization</option>
        <option value="Other">Other</option>
        <option value="Other Resource Hubs">Other Resource Hubs</option>
        <option value="Podcast">Podcast</option>
        <option value="Publication">Publication</option>
        <option value="Scholarship">Scholarship</option>
        <option value="Toolkit">Toolkit</option>
        <option value="Videos and Webinars">Videos and Webinars</option>
        <option value="Zine">Zine</option>
      </select>
      </div>
      <div class="view-toggle">
        <button @click="toggleViewMode">Toggle View ({{ viewMode }})</button>
      </div>
    </div>
    <div class="items-container" :class="{ 'list-view': viewMode === 'list', 'gallery-view': viewMode === 'gallery' }" ref="itemsContainer">
      <div v-for="item in paginatedItems" :key="item.id" class="item">
        <div class="res-header">
          <!-- Simplified Category Display -->
      <div :class="['category', categoryClass(item.Categories)]">
        <span v-html="item.Categories"></span>
      </div>
          <div class="icons-container">
            <i class="fa fa-heart icon-heart"></i>
            <i class="fa fa-plus icon-plus"></i>
          </div>
        </div>
        <img v-if="viewMode === 'gallery'" :src="genericImage" class="resourceImg" alt="Example">
        <h2 class="rh-title"><a :href="item.Link" target="_blank">{{ item.Title }}</a></h2>
        <!--<div class="item-description" v-html="truncateContent(item.Content)"></div>-->
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
</div>
</template>

<style>

.main-bg {
  background-color: #f2f2f2;
  padding: 20px 20px 20px 20px;
  margin: 0px;}

.mainbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.mainbar h1 {
  margin-top: 0px;
  padding: 0px;
}

.list-view {
  padding: 20px
}
.list-view .item {
  flex: 0 1 85%;
  /* Each item takes full width in list view */
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
  justify-content: space-around;
  /* This will distribute space around items */
  margin-top: 40px;
}

.item {
  position: relative;
  flex: 0 1 calc(33.333% - 90px);
  /* On large screens, each item takes up roughly 1/3 of the container width minus some margin */
  margin-bottom: 30px;
  /* Adds space between rows */
  padding: 20px;
  font-size: .9em;
  border: 1px solid #e6e6e6;
  background: #ffffff;
  /* add a shadow to the item */
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.1);
}

.item-description {
  margin: 10px 0px 10px 0px;
}

/* On medium screens, make items take half width */
@media (min-width: 768px) and (max-width: 1024px) {
  .item {
    flex: 0 1 calc(50% - 80px);
    /* Each item takes up roughly half of the container width minus some margin */
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

.category {
  display: inline-block;
  padding: 0px 0px;
  margin: 0px 0px 10px 0px;
  border-radius: 4px;
  text-transform: uppercase;
  font-size: 85%;
}

.icons-container {
  position: absolute;
  top: 20px;
  right: 20px;
}

.icon-heart, .icon-plus {
  color: #401338; /* Default color */
  margin-left: 8px;
  transition: color 0.3s; /* Smooth transition for hover effect */
  cursor: pointer;
}

.icon-heart:hover, .icon-plus:hover {
  color: #10402f; /* Hover color */
}

.tag {
  display: inline-block;
  padding: 6px 12px;
  margin: 4px;
  border-radius: 4px;
  color: #222222;
  transition: background-color 0.3s; /* Smooth transition for hover effect */
}

.tag:hover {
  background-color: #D9E026; /* Hover color */
  color: #401338
}

.tag-air-quality {
  background-color: #E57373;
}

.tag-air-quality:hover {
  background-color: #dc4141;
}

.tag-art {
  background-color: #F06292;
}

.tag-climate-adaptation-resilience {
  background-color: #BA68C8;
  color: #FFFFFF;
}

.tag-coaching {
  background-color: #9575CD;
  color: #ffffff;
}

.tag-community-safety {
  background-color: #7986CB;
}

.tag-conservation-biodiversity {
  background-color: #64B5F6;
}

.tag-culture {
  background-color: #4FC3F7;
}

.tag-dei-anti-oppression {
  background-color: #4DD0E1;
}

.tag-economic-justice {
  background-color: #4DB6AC;
}

.tag-emergency-management {
  background-color: #81C784;
}

.tag-energy-justice {
  background-color: #AED581;
}

.tag-environmental-justice {
  background-color: #DCE775;
}

.tag-extreme-heat {
  background-color: #FFF176;
}

.tag-farming-food-justice {
  background-color: #FFD54F;
}

.tag-fossil-fuels {
  background-color: #FFB74D;
}

.tag-fundraising {
  background-color: #FF8A65;
}

.tag-gender-justice {
  background-color: #A1887F;
  color: #ffffff;
}

.tag-green-space {
  background-color: #E0E0E0;
}

.tag-health-healing {
  background-color: #90A4AE;
  color: #ffffff;
}

.tag-history {
  background-color: #81D4FA;
}

.tag-housing-tenant-justice {
  background-color: #80CBC4;
}

.tag-housing-justice-tenant-support {
  background-color: #A5D6A7;
}

.tag-indigenous-sovereignty {
  background-color: #C5E1A5;
}

.tag-just-recovery {
  background-color: #FFCC80;
}

.tag-just-transition-framework {
  background-color: #FFAB91;
}

.tag-land-justice {
  background-color: #BCAAA4;
  color: #222222;
}

.tag-legal {
  background-color: #B0BEC5;
}

.tag-organizing {
  background-color: #B39DDB;
}

.tag-policy {
  background-color: #9FA8DA;
  color: #ffffff;
}

.tag-pollution {
  background-color: #90CAF9;
}

.tag-popular-education {
  background-color: #81C784;
}

.tag-public-health {
  background-color: #AED581;
}

.tag-rest {
  background-color: #DCE775;
}

.tag-sea-level-rise-flooding {
  background-color: #FFF176;
}

.tag-stories-justice-in-action {
  background-color: #FFD54F;
}

.tag-sustainable-buildings {
  background-color: #FFB74D;
}

.tag-technology-justice {
  background-color: #FF8A65;
}

.tag-transportation {
  background-color: #A1887F;
  color: #ffffff;
}

.tag-videos-webinars {
  background-color: #E0E0E0;
}

.tag-water-equity {
  background-color: #90A4AE;
  color: #ffffff;
}

.tag-youth {
  background-color: #81D4FA;
}

.tag-default {
  background-color: #d9d9d9;
  font-color: #222222;
}

/* Default class for any category not explicitly defined */


.label {
  display: inline-block;
  /* Ensures the label respects the width setting */
  width: 35px;
  text-align: left;
  /* Right-aligns the text, making it look tidy */
  margin-bottom: 10px;
  font-size: 100%;
  font-weight: bold;
}


/* For small screens, make items take full width */
@media (max-width: 768px) {
  .item {
    flex: 0 1 100%;
    /* Each item takes full width of the container */
  }
}

/* For medium screens, make items take half width */
@media (min-width: 768px) and (max-width: 1024px) {
  .item {
    flex: 0 1 calc(50% - 80px);
    /* Each item takes up roughly half of the container width minus some margin */
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
.toolbar {
  display: inline;
  justify-content: left;
  align-items: left;
}
.search-field {
  display: inline;
}

.search-field input {
  padding: 10px 10px 10px 10px;
  font-size: 1em;
  font-weight: bold;
  cursor: pointer;
  width: 150px;
  margin-right: 10px;
  margin-left: 10px;
}

.cat-dropdown {
  display: inline;
}

.view-toggle {
  display: inline;
  float: right;
}

.view-toggle {
  display: inline;
  float: right;
}

.cat-dropdown {
  display: inline;
}

.cat-dropdown select {
  padding: 10px 20px 10px 10px;
  font-size: 1em;
  font-weight: bold;
  cursor: pointer;
  width: 200px;
  margin-left: 20px;
}

.view-toggle button {
  padding: 10px 10px;
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
      filterCategoryType: '', // Represents no category filter selected
      filterText: '', // Represents no text filter applied
    };
  },
  computed: {
    filteredItems() {
    return this.items.filter(item => {
      const itemCategories = item.Categories ? item.Categories.split('|') : [];
      console.log(itemCategories); // Debugging line to inspect categories
      
      const matchesFilterText = !this.filterText || item.Title.toLowerCase().includes(this.filterText.toLowerCase());
      
      const matchesFilterCategoryType = !this.filterCategoryType || itemCategories.includes(this.filterCategoryType);

      return matchesFilterText && matchesFilterCategoryType;
    });
  },
    paginatedItems() {
    const start = (this.currentPage - 1) * this.itemsPerPage;
    const end = start + this.itemsPerPage;
    return this.filteredItems.slice(start, end);
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
          container.scrollIntoView({
            behavior: 'smooth'
          });
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
        'Clinics and Trainings': 'category-clinics-trainings',
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
        'Videos and Webinars': 'category-videos-webinars',
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
        'Videos and Webinars': 'tag-videos-webinars',
        'Water Equity': 'tag-water-equity',
        'Youth': 'tag-youth'
      };
      // Normalize the tag to handle '&', spaces, and other characters for CSS class naming
      return tagClasses[tag] || 'tag-default';
    },
    toggleViewMode() {
      this.viewMode = this.viewMode === 'gallery' ? 'list' : 'gallery';
    },
    truncateContent(content) {
      if (content.length > 55) {
        return content.substring(0, 100) + '...';
      }
      return content;
    },
    decodeHtml(html) {
    var txt = document.createElement("textarea");
    txt.innerHTML = html;
    return txt.value;
  },
  },
  mounted() {
    this.fetchData();
  },
};
</script>