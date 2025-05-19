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
      <div class="projects-complaints-container">
        <!-- Top Navigation Tabs -->
        <div class="tabs-container">
          <button 
            :class="['tab-button', { active: activeTab === 'projects' }]"
            @click="activeTab = 'projects'"
          >
            <i class="fas fa-project-diagram"></i>
            Projects
          </button>
          <button 
            :class="['tab-button', { active: activeTab === 'complaints' }]"
            @click="activeTab = 'complaints'"
          >
            <i class="fas fa-exclamation-circle"></i>
            Complaints
          </button>
        </div>

        <!-- Content Area -->
        <div class="content-area">
          <!-- Projects Tab -->
          <div v-if="activeTab === 'projects'" class="tab-content">
            <div class="action-bar">
              <div class="search-filter">
                <input 
                  type="text" 
                  v-model="projectSearch" 
                  placeholder="Search projects..."
                  class="search-input"
                >
                <select v-model="projectDepartment" class="filter-select">
                  <option value="">All Departments</option>
                  <option v-for="dept in departments" :key="dept" :value="dept">
                    {{ dept }}
                  </option>
                </select>
                <select v-model="projectStatus" class="filter-select">
                  <option value="">All Status</option>
                  <option value="planned">Planned</option>
                  <option value="in-progress">In Progress</option>
                  <option value="completed">Completed</option>
                </select>
              </div>
              <button class="add-button" @click="showAddProjectModal = true">
                <i class="fas fa-plus"></i>
                Add Project
              </button>
            </div>

            <!-- Projects Table -->
            <div class="table-container">
              <table class="data-table">
                <thead>
                  <tr>
                    <th>Project ID</th>
                    <th>Title</th>
                    <th>Department</th>
                    <th>Status</th>
                    <th>Start Date</th>
                    <th>End Date</th>
                    <th>Actions</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="project in filteredProjects" :key="project.id">
                    <td>{{ project.id }}</td>
                    <td>{{ project.title }}</td>
                    <td>{{ project.department }}</td>
                    <td>
                      <span :class="['status-badge', project.status]">
                        {{ project.status }}
                      </span>
                    </td>
                    <td>{{ formatDate(project.start_date) }}</td>
                    <td>{{ formatDate(project.end_date) }}</td>
                    <td>
                      <button class="action-icon" @click="viewProject(project)">
                        <i class="fas fa-eye"></i>
                      </button>
                      <button class="action-icon" @click="editProject(project)">
                        <i class="fas fa-edit"></i>
                      </button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>

          <!-- Complaints Tab -->
          <div v-if="activeTab === 'complaints'" class="tab-content">
            <div class="action-bar">
              <div class="search-filter">
                <input 
                  type="text" 
                  v-model="complaintSearch" 
                  placeholder="Search complaints..."
                  class="search-input"
                >
                <select v-model="complaintDepartment" class="filter-select">
                  <option value="">All Departments</option>
                  <option v-for="dept in departments" :key="dept" :value="dept">
                    {{ dept }}
                  </option>
                </select>
                <select v-model="complaintStatus" class="filter-select">
                  <option value="">All Status</option>
                  <option value="unresolved">Unresolved</option>
                  <option value="assigned">Assigned</option>
                  <option value="resolved">Resolved</option>
                </select>
              </div>
            </div>

            <!-- Complaints Table -->
            <div class="table-container">
              <table class="data-table">
                <thead>
                  <tr>
                    <th>Complaint ID</th>
                    <th>Description</th>
                    <th>Department</th>
                    <th>Status</th>
                    <th>Priority</th>
                    <th>Date Submitted</th>
                    <th>Actions</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="complaint in filteredComplaints" :key="complaint.id">
                    <td>{{ complaint.id }}</td>
                    <td>{{ complaint.description }}</td>
                    <td>{{ complaint.department }}</td>
                    <td>
                      <span :class="['status-badge', complaint.status]">
                        {{ complaint.status }}
                      </span>
                    </td>
                    <td>
                      <span :class="['priority-badge', complaint.priority]">
                        {{ complaint.priority }}
                      </span>
                    </td>
                    <td>{{ formatDate(complaint.date_submitted) }}</td>
                    <td>
                      <button class="action-icon" @click="viewComplaint(complaint)">
                        <i class="fas fa-eye"></i>
                      </button>
                      <button class="action-icon" @click="assignComplaint(complaint)">
                        <i class="fas fa-user-plus"></i>
                      </button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Modals -->
    <ProjectModal 
      v-if="showProjectModal"
      :project="selectedProject"
      :departments="departments"
      @close="showProjectModal = false"
      @save="saveProject"
    />

    <ComplaintModal 
      v-if="showComplaintModal"
      :complaint="selectedComplaint"
      :isCommissioner="true"
      @close="showComplaintModal = false"
      @update-status="updateComplaintStatus"
      @assign="assignComplaint"
    />

    <AssignOfficerModal 
      v-if="showAssignModal"
      :complaint="selectedComplaint"
      :departments="departments"
      :officers="officers"
      @close="showAssignModal = false"
      @assign="assignOfficer"
    />
  </div>
</template>

<script>
import ProjectModal from '../components/ProjectModal.vue'
import ComplaintModal from '../components/ComplaintModal.vue'
import AssignOfficerModal from '../components/AssignOfficerModal.vue'

export default {
  name: 'ProjectsAndComplaints',
  components: {
    ProjectModal,
    ComplaintModal,
    AssignOfficerModal
  },
  data() {
    return {
      activeTab: 'projects',
      projectSearch: '',
      projectDepartment: '',
      projectStatus: '',
      complaintSearch: '',
      complaintDepartment: '',
      complaintStatus: '',
      showProjectModal: false,
      showComplaintModal: false,
      showAssignModal: false,
      showAddProjectModal: false,
      selectedProject: null,
      selectedComplaint: null,
      departments: [
        'Water Supply',
        'Roads & Bridges',
        'Sanitation',
        'Electricity',
        'Public Works'
      ],
      projects: [
        {
          id: 'PRJ1234',
          title: 'Road Widening - MG Road',
          department: 'Roads & Bridges',
          status: 'in-progress',
          start_date: '2024-02-01',
          end_date: '2024-04-30',
          description: 'Widening of MG Road from 2 lanes to 4 lanes',
          budget: 2500000,
          assigned_engineers: ['John Doe', 'Jane Smith']
        }
      ],
      complaints: [
        {
          id: 'CMP4567',
          description: 'Water leakage in Sector 15',
          department: 'Water Supply',
          status: 'unresolved',
          priority: 'high',
          date_submitted: '2024-02-20',
          citizen_name: 'Rahul Sharma',
          contact: '9876543210'
        }
      ],
      officers: [
        {
          id: 'OFF001',
          name: 'Rajesh Kumar',
          role: 'Senior Engineer',
          department: 'Water Supply',
          status: 'available',
          active_cases: 3
        },
        {
          id: 'OFF002',
          name: 'Priya Sharma',
          role: 'Civil Engineer',
          department: 'Roads & Bridges',
          status: 'busy',
          active_cases: 5
        },
        {
          id: 'OFF003',
          name: 'Amit Patel',
          role: 'Sanitation Officer',
          department: 'Sanitation',
          status: 'available',
          active_cases: 2
        },
        {
          id: 'OFF004',
          name: 'Sneha Reddy',
          role: 'Electrical Engineer',
          department: 'Electricity',
          status: 'offline',
          active_cases: 4
        },
        {
          id: 'OFF005',
          name: 'Vikram Singh',
          role: 'Project Manager',
          department: 'Public Works',
          status: 'available',
          active_cases: 1
        }
      ]
    }
  },
  computed: {
    filteredProjects() {
      return this.projects.filter(project => {
        const matchesSearch = project.title.toLowerCase().includes(this.projectSearch.toLowerCase()) ||
                            project.id.toLowerCase().includes(this.projectSearch.toLowerCase())
        const matchesDepartment = !this.projectDepartment || project.department === this.projectDepartment
        const matchesStatus = !this.projectStatus || project.status === this.projectStatus
        return matchesSearch && matchesDepartment && matchesStatus
      })
    },
    filteredComplaints() {
      return this.complaints.filter(complaint => {
        const matchesSearch = complaint.description.toLowerCase().includes(this.complaintSearch.toLowerCase()) ||
                            complaint.id.toLowerCase().includes(this.complaintSearch.toLowerCase())
        const matchesDepartment = !this.complaintDepartment || complaint.department === this.complaintDepartment
        const matchesStatus = !this.complaintStatus || complaint.status === this.complaintStatus
        return matchesSearch && matchesDepartment && matchesStatus
      })
    }
  },
  methods: {
    formatDate(date) {
      return new Date(date).toLocaleDateString('en-IN')
    },
    viewProject(project) {
      this.selectedProject = project
      this.showProjectModal = true
    },
    editProject(project) {
      this.selectedProject = { ...project }
      this.showProjectModal = true
    },
    viewComplaint(complaint) {
      this.selectedComplaint = complaint
      this.showComplaintModal = true
    },
    assignComplaint(complaint) {
      this.selectedComplaint = complaint
      this.showAssignModal = true
    },
    saveProject(project) {
      const index = this.projects.findIndex(p => p.id === project.id)
      if (index !== -1) {
        this.projects[index] = project
      } else {
        this.projects.push({
          ...project,
          id: `PRJ${Math.floor(Math.random() * 10000)}`
        })
      }
      this.showProjectModal = false
    },
    saveComplaint(complaint) {
      const index = this.complaints.findIndex(c => c.id === complaint.id)
      if (index !== -1) {
        this.complaints[index] = complaint
      } else {
        this.complaints.push({
          ...complaint,
          id: `CMP${Math.floor(Math.random() * 10000)}`
        })
      }
      this.showComplaintModal = false
    },
    assignOfficer(assignment) {
      const complaint = this.complaints.find(c => c.id === assignment.complaintId)
      if (complaint) {
        complaint.assigned_officer = assignment.officerName
        complaint.status = 'assigned'
      }
      this.showAssignModal = false
    },
    updateComplaintStatus(update) {
      const complaint = this.complaints.find(c => c.id === update.complaintId)
      if (complaint) {
        complaint.status = update.newStatus
      }
    },
    handleLogout() {
      localStorage.removeItem('isAuthenticated')
      localStorage.removeItem('user')
      this.$router.push('/')
    }
  },
  created() {
    // Set active tab based on URL query parameter
    const tab = this.$route.query.tab
    if (tab === 'projects' || tab === 'complaints') {
      this.activeTab = tab
    }
  },
  watch: {
    // Watch for changes in the URL query parameters
    '$route.query.tab'(newTab) {
      if (newTab === 'projects' || newTab === 'complaints') {
        this.activeTab = newTab
      }
    }
  }
}
</script>

<style scoped>
/* Inherit styles from page-03.vue */
.page03-container10 {
  width: 100%;
  min-height: 100vh;
  background: url('https://images.unsplash.com/photo-1477959858617-67f85cf4f1df?auto=format&fit=crop&q=80') center/cover no-repeat fixed;
  position: relative;
  display: flex;
  overflow: hidden;
}

.page03-container10::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.97) 0%, rgba(240, 245, 255, 0.97) 100%);
  backdrop-filter: blur(2px);
  z-index: 0;
}

/* Main Content Styles */
.main-content {
  margin-left: 260px;
  padding: 2rem;
  width: calc(100% - 260px);
  min-height: 100vh;
  position: relative;
  z-index: 1;
}

/* Projects & Complaints Container */
.projects-complaints-container {
  background: #ffffff;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  padding: 2rem;
}

/* Tabs Container */
.tabs-container {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
  border-bottom: 2px solid rgba(37, 99, 235, 0.1);
  padding-bottom: 1rem;
}

.tab-button {
  padding: 0.8rem 1.5rem;
  border: none;
  background: none;
  color: #4b5563;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.tab-button:hover {
  background: rgba(37, 99, 235, 0.05);
  color: #2563eb;
}

.tab-button.active {
  background: rgba(37, 99, 235, 0.1);
  color: #2563eb;
}

/* Action Bar */
.action-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.5rem;
}

.search-filter {
  display: flex;
  gap: 1rem;
  flex: 1;
}

.search-input {
  padding: 0.6rem 1rem;
  border: 1px solid rgba(37, 99, 235, 0.2);
  border-radius: 8px;
  font-size: 0.9rem;
  width: 300px;
}

.filter-select {
  padding: 0.6rem 1rem;
  border: 1px solid rgba(37, 99, 235, 0.2);
  border-radius: 8px;
  font-size: 0.9rem;
  background: #ffffff;
}

.add-button {
  padding: 0.6rem 1.2rem;
  background: #2563eb;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  font-size: 0.9rem;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  transition: all 0.3s ease;
}

.add-button:hover {
  background: #1d4ed8;
}

/* Table Styles */
.table-container {
  overflow-x: auto;
  border: 1px solid rgba(37, 99, 235, 0.2);
  border-radius: 8px;
}

.data-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 0.9rem;
}

.data-table th,
.data-table td {
  padding: 1rem;
  text-align: left;
  border-bottom: 1px solid rgba(37, 99, 235, 0.1);
}

.data-table th {
  background: rgba(37, 99, 235, 0.05);
  font-weight: 600;
  color: #1e40af;
}

/* Status Badges */
.status-badge {
  padding: 0.3rem 0.8rem;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 500;
}

.status-badge.planned {
  background: rgba(234, 179, 8, 0.1);
  color: #b45309;
}

.status-badge.in-progress {
  background: rgba(37, 99, 235, 0.1);
  color: #1e40af;
}

.status-badge.completed {
  background: rgba(34, 197, 94, 0.1);
  color: #15803d;
}

.status-badge.unresolved {
  background: rgba(239, 68, 68, 0.1);
  color: #dc2626;
}

.status-badge.assigned {
  background: rgba(234, 179, 8, 0.1);
  color: #b45309;
}

.status-badge.resolved {
  background: rgba(34, 197, 94, 0.1);
  color: #15803d;
}

/* Priority Badges */
.priority-badge {
  padding: 0.3rem 0.8rem;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 500;
}

.priority-badge.high {
  background: rgba(239, 68, 68, 0.1);
  color: #dc2626;
}

.priority-badge.medium {
  background: rgba(234, 179, 8, 0.1);
  color: #b45309;
}

.priority-badge.low {
  background: rgba(34, 197, 94, 0.1);
  color: #15803d;
}

/* Action Icons */
.action-icon {
  padding: 0.4rem;
  background: none;
  border: none;
  color: #4b5563;
  cursor: pointer;
  border-radius: 4px;
  transition: all 0.3s ease;
}

.action-icon:hover {
  background: rgba(37, 99, 235, 0.1);
  color: #2563eb;
}

/* Responsive Design */
@media (max-width: 768px) {
  .main-content {
    margin-left: 0;
    width: 100%;
    padding: 1rem;
  }

  .search-filter {
    flex-direction: column;
  }

  .search-input {
    width: 100%;
  }

  .action-bar {
    flex-direction: column;
    gap: 1rem;
  }

  .add-button {
    width: 100%;
  }
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
  overflow-y: auto;
}

.sidebar-header {
  text-align: center;
  margin-bottom: 1.5rem;
  padding-bottom: 1rem;
  border-bottom: 2px solid rgba(37, 99, 235, 0.2);
}

.page03-text10 {
  color: #2563eb;
  font-size: 1.5rem;
  margin-bottom: 1rem;
  font-weight: 800;
  background: linear-gradient(45deg, #2563eb, #3b82f6);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  letter-spacing: 1px;
  position: relative;
  display: inline-block;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  background-color: rgba(37, 99, 235, 0.05);
  border: 2px solid rgba(37, 99, 235, 0.2);
}

.page03-container12 {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  border: 2px solid rgba(37, 99, 235, 0.3);
  margin: 0.8rem auto;
  overflow: hidden;
  box-shadow: 0 0 15px rgba(37, 99, 235, 0.2);
  transition: all 0.3s ease;
}

.page03-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.page03-text11 {
  color: #4b5563;
  font-size: 0.9rem;
  margin-bottom: 0.3rem;
  display: block;
}

.page03-text12 {
  color: #1e40af;
  font-size: 1.2rem;
  font-weight: 700;
  margin-bottom: 0.3rem;
  display: block;
}

.user-role {
  color: #4b5563;
  font-size: 0.9rem;
  font-weight: 500;
  background: rgba(37, 99, 235, 0.1);
  padding: 0.4rem 0.8rem;
  border-radius: 20px;
  display: inline-block;
  margin-top: 0.3rem;
  border: 1px solid rgba(37, 99, 235, 0.2);
}

.sidebar-nav {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin-top: 0.8rem;
}

.page03-link1,
.page03-link2,
.page03-link3,
.page03-link4,
.page03-link6 {
  color: #4b5563;
  text-decoration: none;
  font-size: 0.9rem;
  padding: 0.6rem 0.8rem;
  border-radius: 8px;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  font-weight: 500;
  position: relative;
  border: 1px solid rgba(37, 99, 235, 0.1);
  background: #ffffff;
}

.page03-link1 i,
.page03-link2 i,
.page03-link3 i,
.page03-link4 i,
.page03-link6 i {
  width: 20px;
  text-align: center;
  font-size: 1rem;
  transition: all 0.3s ease;
  color: #2563eb;
}

.page03-link1:hover,
.page03-link2:hover,
.page03-link3:hover,
.page03-link4:hover {
  background: rgba(37, 99, 235, 0.05);
  color: #2563eb;
  transform: translateX(5px);
  border-color: rgba(37, 99, 235, 0.3);
}

.page03-link6 {
  margin-top: auto;
  color: #dc2626;
  border: 1px solid rgba(220, 38, 38, 0.2);
  background: rgba(220, 38, 38, 0.02);
}

.page03-link6:hover {
  background: rgba(220, 38, 38, 0.05);
  color: #dc2626;
  transform: translateX(5px);
  border-color: rgba(220, 38, 38, 0.3);
}

.router-link-active {
  background: rgba(37, 99, 235, 0.1);
  color: #2563eb;
  font-weight: 600;
  border-color: rgba(37, 99, 235, 0.3);
}
</style> 