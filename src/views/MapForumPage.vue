<template>
  <div class="page03-container10">
    <!-- Sidebar -->
    <div class="page03-container11">
      <div class="sidebar-header">
        <span class="page03-text10">&lt;/&gt; Civilinc</span>
        <div class="page03-container12">
          <img src="/Shivayogi-pic.jpg" alt="Profile" class="page03-image" />
        </div>
        <span class="page03-text11">Welcome back,</span>
        <span class="page03-text12">Dr.Shivayogi.B.Yali</span>
        <span class="user-role">Municipal Commissioner</span>
      </div>
      
      <nav class="sidebar-nav">
        <router-link to="/dashboard" class="page03-link1">
          <i class="fas fa-chart-line"></i>
          Dashboard
        </router-link>
        <router-link to="/projects-complaints" class="page03-link2">
          <i class="fas fa-tasks"></i>
          Projects & Complaints
        </router-link>
        <router-link to="/map-forum" class="page03-link3">
          <i class="fas fa-map-marked-alt"></i>
          Map & Forum
        </router-link>
        <router-link to="/data-reports" class="page03-link4">
          <i class="fas fa-database"></i>
          Data & Reports
        </router-link>
        <router-link to="/" class="page03-link6" @click="handleLogout">
          <i class="fas fa-sign-out-alt"></i>
          Logout
        </router-link>
      </nav>
    </div>

    <!-- Main Content -->
    <div class="main-content">
      <div class="map-forum-container">
        <!-- Map Panel -->
        <div class="map-panel">
          <div class="map-controls">
            <div class="view-toggle">
              <button 
                :class="['toggle-btn', { active: viewMode === 'projects' }]"
                @click="viewMode = 'projects'"
              >
                <i class="fas fa-project-diagram"></i>
                Projects
              </button>
              <button 
                :class="['toggle-btn', { active: viewMode === 'complaints' }]"
                @click="viewMode = 'complaints'"
              >
                <i class="fas fa-exclamation-circle"></i>
                Complaints
              </button>
            </div>
            
            <div class="filters">
              <select v-model="selectedDepartment" class="filter-select">
                <option value="">All Departments</option>
                <option v-for="dept in departments" :key="dept" :value="dept">
                  {{ dept }}
                </option>
              </select>
              
              <select v-model="selectedZone" class="filter-select">
                <option value="">All Zones</option>
                <option v-for="zone in zones" :key="zone" :value="zone">
                  {{ zone }}
                </option>
              </select>
            </div>
          </div>

          <div id="map" class="map-container"></div>
        </div>

        <!-- Forum Panel -->
        <div class="forum-panel">
          <div class="forum-header">
            <h2>Department Forum</h2>
            <select v-model="selectedForumDepartment" class="forum-department-select">
              <option v-for="dept in departments" :key="dept" :value="dept">
                {{ dept }}
              </option>
            </select>
          </div>

          <div class="forum-tabs">
            <button 
              :class="['forum-tab', { active: forumView === 'recent' }]"
              @click="forumView = 'recent'"
            >
              Most Recent
            </button>
            <button 
              :class="['forum-tab', { active: forumView === 'active' }]"
              @click="forumView = 'active'"
            >
              Most Active
            </button>
            <button 
              :class="['forum-tab', { active: forumView === 'mentions' }]"
              @click="forumView = 'mentions'"
            >
              Mentions
            </button>
          </div>

          <div class="forum-threads">
            <div v-for="thread in filteredThreads" :key="thread.id" class="thread-card">
              <div class="thread-header">
                <h3>{{ thread.title }}</h3>
                <span class="thread-meta">
                  {{ thread.department }} • {{ formatDate(thread.timestamp) }}
                </span>
              </div>
              <p class="thread-preview">{{ thread.content }}</p>
              <div class="thread-footer">
                <span class="author">By {{ thread.created_by }}</span>
                <span class="comments-count">
                  <i class="fas fa-comments"></i>
                  {{ thread.comments.length }}
                </span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- New Thread Modal -->
    <div v-if="showNewThreadModal" class="modal-overlay">
      <div class="modal-content">
        <div class="modal-header">
          <h3>New Thread</h3>
          <button class="close-btn" @click="showNewThreadModal = false">
            <i class="fas fa-times"></i>
          </button>
        </div>
        <div class="modal-body">
          <input 
            type="text" 
            v-model="newThread.title" 
            placeholder="Thread Title"
            class="thread-title-input"
          >
          <textarea 
            v-model="newThread.content" 
            placeholder="Write your message..."
            class="thread-content-input"
          ></textarea>
          <div class="attachment-section">
            <button class="attach-btn">
              <i class="fas fa-paperclip"></i>
              Attach Files
            </button>
            <input type="file" multiple class="file-input" @change="handleFileUpload">
          </div>
        </div>
        <div class="modal-footer">
          <button class="cancel-btn" @click="showNewThreadModal = false">Cancel</button>
          <button class="submit-btn" @click="createThread">Create Thread</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'

export default {
  name: 'MapForumPage',
  data() {
    return {
      viewMode: 'projects',
      selectedDepartment: '',
      selectedZone: '',
      selectedForumDepartment: 'Water',
      forumView: 'recent',
      showNewThreadModal: false,
      mapLayers: {
        roads: true,
        waterLines: false,
        zones: true
      },
      departments: [
        'Water Supply',
        'Roads & Bridges',
        'Sanitation',
        'Electricity',
        'Public Works'
      ],
      zones: [
        'Ward 1',
        'Ward 2',
        'Ward 3',
        'Ward 4',
        'Ward 5'
      ],
      map: null,
      markers: [],
      newThread: {
        title: '',
        content: '',
        attachments: []
      },
      forumThreads: [
        {
          id: 'THR001',
          department: 'Water Supply',
          title: 'Pipeline delay at Ward 9',
          content: 'Need urgent attention for the pipeline project delay. Multiple complaints received.',
          created_by: 'Eng. Mehta',
          timestamp: '2024-02-20T14:00',
          comments: [
            {
              author: 'Commissioner Rao',
              content: 'Escalating this to priority. Tagging @CivilDept',
              timestamp: '2024-02-20T14:30',
              attachments: ['report.pdf'],
              mentions: ['CivilDept']
            }
          ]
        }
      ],
      isAuthenticated: false
    }
  },
  created() {
    // Check authentication
    this.isAuthenticated = localStorage.getItem('isAuthenticated') === 'true'
    if (!this.isAuthenticated) {
      this.$router.push('/sign-in')
      return
    }
  },
  mounted() {
    if (!this.isAuthenticated) return

    // Initialize map after component is mounted
    this.$nextTick(() => {
      this.initMap()
    })
  },
  beforeDestroy() {
    if (this.map) {
      this.map.remove()
      this.map = null
    }
  },
  computed: {
    filteredThreads() {
      return this.forumThreads.filter(thread => {
        const matchesDepartment = thread.department === this.selectedForumDepartment
        const matchesView = this.forumView === 'recent' || 
                          (this.forumView === 'active' && thread.comments.length > 2) ||
                          (this.forumView === 'mentions' && thread.comments.some(c => c.mentions?.length > 0))
        return matchesDepartment && matchesView
      })
    }
  },
  methods: {
    initMap() {
      try {
        // Fix Leaflet default icon issue
        delete L.Icon.Default.prototype._getIconUrl
        L.Icon.Default.mergeOptions({
          iconRetinaUrl: require('leaflet/dist/images/marker-icon-2x.png'),
          iconUrl: require('leaflet/dist/images/marker-icon.png'),
          shadowUrl: require('leaflet/dist/images/marker-shadow.png')
        })

        // Initialize map
        const mapElement = document.getElementById('map')
        if (!mapElement) {
          console.error('Map element not found')
          return
        }

        // Create map instance
        this.map = L.map('map', {
          center: [12.9716, 77.5946],
          zoom: 12,
          zoomControl: true,
          attributionControl: true
        })

        // Add tile layer
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
          attribution: '© OpenStreetMap contributors'
        }).addTo(this.map)

        // Add sample markers
        this.addSampleMarkers()

        // Force a resize event
        setTimeout(() => {
          this.map.invalidateSize()
        }, 100)
      } catch (error) {
        console.error('Error initializing map:', error)
      }
    },
    addSampleMarkers() {
      const markers = [
        {
          type: 'project',
          coords: [12.94, 77.61],
          title: 'Water Pipeline Upgrade',
          description: 'Ongoing water pipeline upgrade project'
        },
        {
          type: 'complaint',
          coords: [12.95, 77.62],
          title: 'Road Repair Needed',
          description: 'Pothole reported on main road'
        }
      ]

      markers.forEach(marker => {
        L.marker(marker.coords)
          .bindPopup(`
            <div class="marker-popup">
              <h4>${marker.title}</h4>
              <p>${marker.description}</p>
            </div>
          `)
          .addTo(this.map)
      })
    },
    formatDate(date) {
      return new Date(date).toLocaleDateString('en-IN', {
        year: 'numeric',
        month: 'short',
        day: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
      })
    },
    handleFileUpload(event) {
      const files = Array.from(event.target.files)
      this.newThread.attachments.push(...files)
    },
    createThread() {
      if (!this.newThread.title || !this.newThread.content) return

      const thread = {
        id: `THR${Math.floor(Math.random() * 10000)}`,
        department: this.selectedForumDepartment,
        title: this.newThread.title,
        content: this.newThread.content,
        created_by: 'Dr.Shivayogi.B.Yali',
        timestamp: new Date().toISOString(),
        comments: [],
        attachments: this.newThread.attachments
      }

      this.forumThreads.unshift(thread)
      this.showNewThreadModal = false
      this.newThread = { title: '', content: '', attachments: [] }
    },
    handleLogout() {
      localStorage.removeItem('isAuthenticated')
      localStorage.removeItem('user')
      this.$router.push('/')
    }
  }
}
</script>

<style scoped>
/* Base Container */
.page03-container10 {
  width: 100%;
  min-height: 100vh;
  display: flex;
  background: #f8fafc;
  position: relative;
}

/* Sidebar Styles */
.page03-container11 {
  width: 260px;
  height: 100vh;
  position: fixed;
  left: 0;
  top: 0;
  background: #ffffff;
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.1);
  z-index: 100;
  border-right: 2px solid rgba(37, 99, 235, 0.2);
}

.sidebar-header {
  margin-bottom: 2rem;
}

.page03-text10 {
  font-size: 1.5rem;
  font-weight: bold;
  color: #2563eb;
  margin-bottom: 1rem;
  display: block;
}

.page03-container12 {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  overflow: hidden;
  margin: 1rem 0;
}

.page03-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.page03-text11 {
  font-size: 0.9rem;
  color: #6b7280;
  display: block;
  margin-bottom: 0.25rem;
}

.page03-text12 {
  font-size: 1.2rem;
  font-weight: 600;
  color: #1e40af;
  display: block;
  margin-bottom: 0.25rem;
}

.user-role {
  font-size: 0.9rem;
  color: #4b5563;
  display: block;
}

.sidebar-nav {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.page03-link1,
.page03-link2,
.page03-link3,
.page03-link4,
.page03-link6 {
  padding: 0.75rem 1rem;
  border-radius: 8px;
  color: #4b5563;
  text-decoration: none;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  transition: all 0.3s ease;
}

.page03-link1:hover,
.page03-link2:hover,
.page03-link3:hover,
.page03-link4:hover,
.page03-link6:hover {
  background: rgba(37, 99, 235, 0.1);
  color: #2563eb;
}

.page03-link1.router-link-active,
.page03-link2.router-link-active,
.page03-link3.router-link-active,
.page03-link4.router-link-active,
.page03-link6.router-link-active {
  background: rgba(37, 99, 235, 0.1);
  color: #2563eb;
  font-weight: 500;
}

/* Main Content */
.main-content {
  margin-left: 260px;
  padding: 2rem;
  width: calc(100% - 260px);
  min-height: 100vh;
  position: relative;
  z-index: 1;
}

/* Map & Forum Container */
.map-forum-container {
  display: grid;
  grid-template-columns: 60% 40%;
  gap: 2rem;
  height: calc(100vh - 4rem);
  background: #ffffff;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  position: relative;
  padding: 1rem;
}

/* Map Panel */
.map-panel {
  display: flex;
  flex-direction: column;
  background: #f8fafc;
  border-radius: 8px;
  overflow: hidden;
  height: 100%;
}

.map-controls {
  padding: 1rem;
  background: #ffffff;
  border-bottom: 1px solid rgba(37, 99, 235, 0.1);
}

/* Map Container */
.map-container {
  flex: 1;
  width: 100%;
  height: 100%;
  min-height: 400px;
  position: relative;
  z-index: 1;
  background: #f8fafc;
}

/* Forum Panel */
.forum-panel {
  display: flex;
  flex-direction: column;
  background: #ffffff;
  border-radius: 8px;
  overflow: hidden;
  height: 100%;
}

.forum-header {
  padding: 1rem;
  border-bottom: 1px solid rgba(37, 99, 235, 0.1);
}

.forum-header h2 {
  font-size: 1.2rem;
  color: #1e40af;
  margin: 0;
}

.forum-tabs {
  padding: 1rem;
  border-bottom: 1px solid rgba(37, 99, 235, 0.1);
  display: flex;
  gap: 1rem;
}

.forum-tab {
  padding: 0.5rem 1rem;
  border: none;
  background: none;
  color: #4b5563;
  cursor: pointer;
  border-radius: 6px;
}

.forum-tab.active {
  background: rgba(37, 99, 235, 0.1);
  color: #2563eb;
}

.forum-threads {
  flex: 1;
  overflow-y: auto;
  padding: 1rem;
}

.thread-card {
  background: #f8fafc;
  border: 1px solid rgba(37, 99, 235, 0.1);
  border-radius: 8px;
  padding: 1rem;
  margin-bottom: 1rem;
}

.thread-header h3 {
  font-size: 1rem;
  color: #1e40af;
  margin: 0 0 0.3rem 0;
}

.thread-meta {
  font-size: 0.8rem;
  color: #6b7280;
}

.thread-preview {
  font-size: 0.9rem;
  color: #4b5563;
  margin: 0.5rem 0;
}

.thread-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.8rem;
  color: #6b7280;
}

/* Controls and Buttons */
.view-toggle {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.toggle-btn {
  padding: 0.5rem 1rem;
  border: 1px solid rgba(37, 99, 235, 0.2);
  border-radius: 6px;
  background: #ffffff;
  color: #4b5563;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.toggle-btn.active {
  background: rgba(37, 99, 235, 0.1);
  color: #2563eb;
}

.filters {
  display: flex;
  gap: 1rem;
}

.filter-select {
  padding: 0.5rem;
  border: 1px solid rgba(37, 99, 235, 0.2);
  border-radius: 6px;
  min-width: 150px;
}

/* Responsive Design */
@media (max-width: 1024px) {
  .map-forum-container {
    grid-template-columns: 1fr;
    height: auto;
  }

  .map-panel,
  .forum-panel {
    height: 50vh;
  }
}

@media (max-width: 768px) {
  .page03-container11 {
    width: 100%;
    height: auto;
    position: relative;
  }

  .main-content {
    margin-left: 0;
    width: 100%;
    padding: 1rem;
  }

  .filters {
    flex-direction: column;
  }

  .filter-select {
    width: 100%;
  }
}

/* Ensure Leaflet map is visible */
:deep(.leaflet-container) {
  width: 100%;
  height: 100%;
  z-index: 1;
}

:deep(.leaflet-control-container) {
  z-index: 2;
}

:deep(.leaflet-pane) {
  z-index: 1;
}

/* Modal Styles */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.modal-content {
  background: #ffffff;
  border-radius: 12px;
  width: 90%;
  max-width: 600px;
  max-height: 90vh;
  display: flex;
  flex-direction: column;
}

.modal-header {
  padding: 1rem;
  border-bottom: 1px solid rgba(37, 99, 235, 0.1);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-header h3 {
  margin: 0;
  color: #1e40af;
}

.close-btn {
  background: none;
  border: none;
  color: #6b7280;
  cursor: pointer;
  font-size: 1.2rem;
}

.modal-body {
  padding: 1rem;
  overflow-y: auto;
}

.thread-title-input {
  width: 100%;
  padding: 0.8rem;
  border: 1px solid rgba(37, 99, 235, 0.2);
  border-radius: 8px;
  font-size: 1rem;
  margin-bottom: 1rem;
}

.thread-content-input {
  width: 100%;
  padding: 0.8rem;
  border: 1px solid rgba(37, 99, 235, 0.2);
  border-radius: 8px;
  font-size: 1rem;
  min-height: 150px;
  resize: vertical;
  margin-bottom: 1rem;
}

.attachment-section {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.attach-btn {
  padding: 0.5rem 1rem;
  background: #f3f4f6;
  border: 1px solid rgba(37, 99, 235, 0.2);
  border-radius: 6px;
  color: #4b5563;
  font-size: 0.9rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.file-input {
  display: none;
}

.modal-footer {
  padding: 1rem;
  border-top: 1px solid rgba(37, 99, 235, 0.1);
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
}

.cancel-btn {
  padding: 0.6rem 1.2rem;
  background: #f3f4f6;
  border: 1px solid rgba(37, 99, 235, 0.2);
  border-radius: 6px;
  color: #4b5563;
  font-size: 0.9rem;
  cursor: pointer;
}

.submit-btn {
  padding: 0.6rem 1.2rem;
  background: #2563eb;
  border: none;
  border-radius: 6px;
  color: #ffffff;
  font-size: 0.9rem;
  cursor: pointer;
}

.submit-btn:hover {
  background: #1d4ed8;
}

/* Marker Styles */
.marker {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.marker.project {
  color: #2563eb;
}

.marker.complaint {
  color: #dc2626;
}

.marker-popup {
  padding: 0.5rem;
}

.marker-popup h4 {
  margin: 0 0 0.5rem 0;
  color: #1e40af;
}

.marker-popup p {
  margin: 0.3rem 0;
  color: #4b5563;
  font-size: 0.9rem;
}
</style> 