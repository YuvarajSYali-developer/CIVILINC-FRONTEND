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
      <!-- Quick Actions -->
      <div class="quick-actions">
        <button class="action-btn" @click="navigateToProjects">
          <i class="fas fa-plus"></i>
          Add Project
        </button>
        <button class="action-btn" @click="$router.push('/map-forum')">
          <i class="fas fa-map-marked-alt"></i>
          View Map
        </button>
        <button class="action-btn" @click="navigateToComplaints">
          <i class="fas fa-list"></i>
          Recent Complaints
        </button>
        <button class="action-btn">
          <i class="fas fa-bell"></i>
          Notifications
        </button>
      </div>

      <!-- KPI Summary Cards -->
    <div class="page03-container13">
        <div class="page03-container14">
          <div class="stat-icon">
            <i class="fas fa-clipboard-list"></i>
        </div>
          <div class="stat-content">
            <span class="page03-text13">Total Projects</span>
            <span class="page03-text14">This Quarter</span>
            <span class="page03-text15">₹2.5 Cr Budget</span>
        </div>
        </div>
        <div class="page03-container15">
          <div class="stat-icon">
            <i class="fas fa-check-circle"></i>
        </div>
          <div class="stat-content">
            <span class="page03-text16">Resolved</span>
            <span class="page03-text18">This Month</span>
            <span class="page03-text17">85% Success Rate</span>
    </div>
  </div>
        <div class="page03-container16">
          <div class="stat-icon">
            <i class="fas fa-clock"></i>
      </div>
          <div class="stat-content">
            <span class="page03-text20">Pending</span>
            <span class="page03-text19">Active Projects</span>
            <span class="page03-text21">12 Projects</span>
    </div>
    </div>
        <div class="page03-container17">
          <div class="stat-icon">
            <i class="fas fa-stopwatch"></i>
    </div>
          <div class="stat-content">
            <span class="page03-text22">Response Time</span>
            <span class="page03-text23">Average</span>
            <span class="page03-text24">2.5 hours</span>
  </div>
      </div>
    </div>

      <!-- Notifications and Activity Feed -->
      <div class="dashboard-grid">
        <!-- Notifications Section -->
        <div class="notifications-section">
          <div class="section-header">
            <span class="section-title">Notifications</span>
            <button class="view-all-btn">View All</button>
  </div>
          <div class="notifications-list">
            <div v-for="(notification, index) in notifications" :key="index" class="notification-item">
              <div class="notification-icon" :class="notification.type">
                <i :class="notification.icon"></i>
      </div>
              <div class="notification-content">
                <p class="notification-text">{{ notification.message }}</p>
                <span class="notification-time">{{ notification.time }}</span>
    </div>
    </div>
          </div>
      </div>
  
        <!-- Activity Feed -->
        <div class="activity-feed">
          <div class="section-header">
            <span class="section-title">Activity Feed</span>
            <button class="view-all-btn">View All</button>
          </div>
          <div class="activity-list">
            <div v-for="(activity, index) in activities" :key="index" class="activity-item">
              <div class="activity-icon">
                <i :class="activity.icon"></i>
              </div>
              <div class="activity-content">
                <p class="activity-text">{{ activity.message }}</p>
                <div class="activity-meta">
                  <span class="activity-time">{{ activity.time }}</span>
                  <span class="activity-department">{{ activity.department }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
  
      <!-- Chart Section -->
      <div class="page03-container18">
        <div class="section-header">
          <span class="page03-text25">Projects by Department</span>
          <div class="chart-legend">
            <div class="legend-item" v-for="(label, index) in chartData.labels" :key="index">
              <span class="legend-color" :style="{ backgroundColor: chartData.datasets[0].backgroundColor[index] }"></span>
              <span class="legend-label">{{ label }}</span>
            </div>
          </div>
        </div>
        <div class="chart-container">
          <canvas id="barChart" height="300"></canvas>
      </div>
    </div>
  
      <!-- Recent Complaints Section -->
      <div class="page03-container20">
        <div class="section-header">
          <span class="page03-text25">Recent Complaints</span>
          <button class="view-all-btn">View All</button>
        </div>
        <div class="recent-complaints">
          <div v-for="(complaint, index) in recentComplaints" :key="index" class="complaint-item">
            <div class="complaint-header">
              <span class="complaint-title">{{ complaint.title }}</span>
              <span :class="['complaint-status', complaint.status]">{{ complaint.status }}</span>
            </div>
            <p class="complaint-description">{{ complaint.description }}</p>
            <div class="complaint-footer">
              <span class="complaint-date">
                <i class="far fa-calendar-alt"></i>
                {{ complaint.date }}
              </span>
              <span class="complaint-category">
                <i class="fas fa-tag"></i>
                {{ complaint.category }}
              </span>
            </div>
          </div>
        </div>
      </div>

      <!-- Officer's Timeline Section -->
      <div class="timeline-section">
        <div class="section-header">
          <span class="page03-text25">Today's Schedule</span>
          <button class="view-all-btn">View Calendar</button>
        </div>
        <div class="timeline-container">
          <div v-for="(event, index) in timelineEvents" :key="index" class="timeline-item" :class="{ 'completed': event.completed }">
            <div class="timeline-time">
              <span class="time">{{ event.time }}</span>
              <div class="timeline-dot"></div>
            </div>
            <div class="timeline-content">
              <div class="timeline-header">
                <h4>{{ event.title }}</h4>
                <span :class="['event-status', event.priority]">{{ event.priority }}</span>
              </div>
              <p class="event-description">{{ event.description }}</p>
              <div class="event-meta">
                <span class="event-location">
                  <i class="fas fa-map-marker-alt"></i>
                  {{ event.location }}
                </span>
                <span class="event-duration">
                  <i class="far fa-clock"></i>
                  {{ event.duration }}
                </span>
              </div>
              <div class="event-actions">
                <button class="action-btn" @click="toggleEventStatus(index)">
                  <i :class="event.completed ? 'fas fa-undo' : 'fas fa-check'"></i>
                  {{ event.completed ? 'Mark Incomplete' : 'Mark Complete' }}
                </button>
                <button class="action-btn" @click="rescheduleEvent(index)">
                  <i class="fas fa-calendar-alt"></i>
                  Reschedule
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import DangerousHTML from 'dangerous-html/vue'
import { Chart, registerables } from 'chart.js'

// Register Chart.js components
Chart.register(...registerables)

export default {
  name: 'Page03',
  props: {},
  components: {
    DangerousHTML,
  },
  data() {
    return {
      rawv1yq: ' ',
      chart: null,
      chartData: {
        labels: ['Roads & Bridges', 'Water Supply', 'Electricity', 'Sanitation', 'Public Works'],
        datasets: [{
          label: 'Projects by Department',
          data: [45, 30, 25, 20, 15],
          backgroundColor: [
            'rgba(0, 255, 135, 0.8)',
            'rgba(96, 239, 255, 0.8)',
            'rgba(255, 99, 132, 0.8)',
            'rgba(54, 162, 235, 0.8)',
            'rgba(255, 206, 86, 0.8)'
          ],
          borderColor: [
            'rgba(0, 255, 135, 1)',
            'rgba(96, 239, 255, 1)',
            'rgba(255, 99, 132, 1)',
            'rgba(54, 162, 235, 1)',
            'rgba(255, 206, 86, 1)'
          ],
          borderWidth: 1
        }]
      },
      notifications: [
        {
          type: 'urgent',
          icon: 'fas fa-exclamation-triangle',
          message: 'Water supply disruption in Sector 15',
          time: '10 minutes ago'
        },
        {
          type: 'update',
          icon: 'fas fa-info-circle',
          message: 'New project approved: Road widening in Main Market',
          time: '1 hour ago'
        },
        {
          type: 'success',
          icon: 'fas fa-check-circle',
          message: 'Sewage treatment plant maintenance completed',
          time: '2 hours ago'
        }
      ],
      activities: [
        {
          icon: 'fas fa-road',
          message: 'Road repair work started on MG Road',
          time: '30 minutes ago',
          department: 'Public Works'
        },
        {
          icon: 'fas fa-water',
          message: 'Water pipeline inspection completed in Sector 12',
          time: '1 hour ago',
          department: 'Water Supply'
        },
        {
          icon: 'fas fa-bolt',
          message: 'Street light maintenance scheduled for tonight',
          time: '2 hours ago',
          department: 'Electricity'
        }
      ],
      recentComplaints: [
        {
          title: 'Pothole on MG Road',
          description: 'Large pothole causing traffic issues near the intersection',
          status: 'pending',
          date: '2024-02-20',
          category: 'Roads'
        },
        {
          title: 'Water Leak in Sector 15',
          description: 'Water pipe leaking near the community park',
          status: 'resolved',
          date: '2024-02-19',
          category: 'Water'
        },
        {
          title: 'Street Light Outage',
          description: 'Multiple street lights not working on Ring Road',
          status: 'in-progress',
          date: '2024-02-18',
          category: 'Electricity'
        }
      ],
      timelineEvents: [
        {
          time: '09:00 AM',
          title: 'Morning Briefing',
          description: 'Daily department meeting to discuss ongoing projects and priorities',
          location: 'Conference Room A',
          duration: '1 hour',
          priority: 'high',
          completed: false
        },
        {
          time: '10:30 AM',
          title: 'Site Inspection - Water Pipeline Project',
          description: 'Inspect the ongoing water pipeline upgrade in Ward 11',
          location: 'Ward 11, Sector 3',
          duration: '2 hours',
          priority: 'high',
          completed: false
        },
        {
          time: '01:00 PM',
          title: 'Lunch with Department Heads',
          description: 'Quarterly review meeting with department heads',
          location: 'City Hall Cafeteria',
          duration: '1 hour',
          priority: 'medium',
          completed: false
        },
        {
          time: '02:30 PM',
          title: 'Public Hearing',
          description: 'Public hearing for the new road widening project',
          location: 'Town Hall',
          duration: '2 hours',
          priority: 'high',
          completed: false
        },
        {
          time: '04:30 PM',
          title: 'Review Project Reports',
          description: 'Review and approve pending project reports',
          location: 'Office',
          duration: '1 hour',
          priority: 'medium',
          completed: false
        }
      ]
    }
  },
  metaInfo: {
    title: 'Dashboard - Civilinc',
    meta: [
      {
        property: 'og:title',
        content: 'Dashboard - Civilinc',
      },
    ],
  },
  mounted() {
    this.$nextTick(() => {
      this.initializeCharts()
    })
  },
  methods: {
    initializeCharts() {
      try {
        const ctx = document.getElementById('barChart')
        if (ctx && !this.chart) {
          this.chart = new Chart(ctx, {
            type: 'bar',
            data: this.chartData,
            options: {
              responsive: true,
              maintainAspectRatio: false,
              scales: {
                y: {
                  beginAtZero: true,
                  grid: {
                    color: 'rgba(255, 255, 255, 0.1)'
                  },
                  ticks: {
                    color: 'rgba(255, 255, 255, 0.7)',
                    font: {
                      size: 12
                    }
                  }
                },
                x: {
                  grid: {
                    color: 'rgba(255, 255, 255, 0.1)'
                  },
                  ticks: {
                    color: 'rgba(255, 255, 255, 0.7)',
                    font: {
                      size: 12
                    }
                  }
                }
              },
              plugins: {
                legend: {
                  display: false
                },
                tooltip: {
                  backgroundColor: 'rgba(0, 0, 0, 0.8)',
                  titleColor: '#fff',
                  bodyColor: '#fff',
                  borderColor: 'rgba(255, 255, 255, 0.1)',
                  borderWidth: 1,
                  padding: 10,
                  displayColors: false
                }
              },
              animation: {
                duration: 2000,
                easing: 'easeInOutQuart'
              }
            }
          })
        }
      } catch (error) {
        console.error('Error initializing charts:', error)
      }
    },
    handleLogout() {
      // Clear any stored authentication data
      localStorage.removeItem('isAuthenticated');
      localStorage.removeItem('user');
      // Navigate to landing page
      this.$router.push('/');
    },
    toggleEventStatus(index) {
      this.timelineEvents[index].completed = !this.timelineEvents[index].completed
    },
    rescheduleEvent(index) {
      // Implement rescheduling logic
      console.log('Rescheduling event:', this.timelineEvents[index].title)
    },
    navigateToProjects() {
      this.$router.push({
        path: '/projects-complaints',
        query: { tab: 'projects' }
      })
    },
    navigateToComplaints() {
      this.$router.push({
        path: '/projects-complaints',
        query: { tab: 'complaints' }
      })
    }
  }
}
</script>

<style scoped>
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
 
/* Enhanced Sidebar Styles */
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

.page03-text10::before {
  content: 'CIVIL';
  position: absolute;
  top: -0.5rem;
  left: 0.5rem;
  font-size: 0.7rem;
  font-weight: 600;
  color: #2563eb;
  background: #ffffff;
  padding: 0 0.3rem;
  border-radius: 4px;
  border: 1px solid rgba(37, 99, 235, 0.2);
}

.page03-text10::after {
  content: 'INC';
  position: absolute;
  bottom: -0.5rem;
  right: 0.5rem;
  font-size: 0.7rem;
  font-weight: 600;
  color: #2563eb;
  background: #ffffff;
  padding: 0 0.3rem;
  border-radius: 4px;
  border: 1px solid rgba(37, 99, 235, 0.2);
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

.page03-container12:hover {
  transform: scale(1.05);
  border-color: rgba(37, 99, 235, 0.5);
  box-shadow: 0 0 40px rgba(37, 99, 235, 0.3);
}

.page03-container12:hover .page03-image {
  transform: scale(1.1);
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

/* Enhanced Navigation Links */
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
.page03-link4:hover,
.page03-link6:hover {
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

/* Active Link Styles */
.router-link-active {
  background: rgba(37, 99, 235, 0.1);
  color: #2563eb;
  font-weight: 600;
  border-color: rgba(37, 99, 235, 0.3);
}

/* Enhanced Card Styles */
.notifications-section,
.activity-feed,
.page03-container18,
.page03-container20,
.page03-container14,
.page03-container15,
.page03-container16,
.page03-container17 {
  background: #ffffff;
  border-radius: 16px;
  padding: 2rem;
  border: 2px solid rgba(37, 99, 235, 0.2);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
}

.notification-item,
.activity-item,
.complaint-item {
  background: #ffffff;
  border-radius: 12px;
  padding: 1.5rem;
  transition: all 0.3s ease;
  border: 1px solid rgba(37, 99, 235, 0.2);
  margin-bottom: 1rem;
}

.notification-item:hover,
.activity-item:hover,
.complaint-item:hover {
  background: rgba(37, 99, 235, 0.02);
  transform: translateX(5px);
  border-color: rgba(37, 99, 235, 0.3);
}

/* Enhanced Quick Actions */
.quick-actions {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
  flex-wrap: nowrap;
  overflow-x: auto;
  padding-bottom: 0.5rem;
}

.action-btn {
  padding: 0.8rem 1.5rem;
  background: #ffffff;
  border: 2px solid rgba(37, 99, 235, 0.2);
  border-radius: 8px;
  color: #1e40af;
  display: flex;
  align-items: center;
  gap: 0.6rem;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 500;
  font-size: 1rem;
  white-space: nowrap;
  min-width: fit-content;
}

.action-btn:hover {
  background: rgba(37, 99, 235, 0.05);
  transform: translateY(-3px);
  border-color: rgba(37, 99, 235, 0.3);
  box-shadow: 0 6px 20px rgba(37, 99, 235, 0.1);
}

/* Section Headers */
.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
  padding-bottom: 1rem;
  border-bottom: 2px solid rgba(37, 99, 235, 0.2);
}

.section-title {
  color: #1e40af;
  font-size: 1.6rem;
  font-weight: 700;
  background: linear-gradient(45deg, #2563eb, #3b82f6);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* View All Button */
.view-all-btn {
  background: rgba(37, 99, 235, 0.05);
  color: #2563eb;
  border: 1px solid rgba(37, 99, 235, 0.2);
  padding: 0.6rem 1.2rem;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 500;
}

.view-all-btn:hover {
  background: rgba(37, 99, 235, 0.1);
  transform: translateY(-2px);
  border-color: rgba(37, 99, 235, 0.3);
}

/* Main Content Area */
.main-content {
  margin-left: 260px;
  padding: 2rem;
  width: calc(100% - 260px);
  min-height: 100vh;
  position: relative;
  z-index: 1;
}

/* Chart Section */
.chart-legend {
  display: flex;
  gap: 1.2rem;
  flex-wrap: wrap;
  margin-top: 1rem;
  padding: 1.2rem;
  background: #ffffff;
  border-radius: 12px;
  border: 1px solid rgba(37, 99, 235, 0.2);
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  padding: 0.5rem 1rem;
  background: rgba(37, 99, 235, 0.05);
  border-radius: 20px;
  border: 1px solid rgba(37, 99, 235, 0.2);
}

/* Status Indicators */
.complaint-status.pending {
  background: rgba(234, 179, 8, 0.1);
  color: #b45309;
  border: 1px solid rgba(234, 179, 8, 0.3);
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-weight: 500;
}

.complaint-status.resolved {
  background: rgba(34, 197, 94, 0.1);
  color: #15803d;
  border: 1px solid rgba(34, 197, 94, 0.3);
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-weight: 500;
}

.complaint-status.in-progress {
  background: rgba(37, 99, 235, 0.1);
  color: #1e40af;
  border: 1px solid rgba(37, 99, 235, 0.3);
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-weight: 500;
}

/* Enhanced Notifications and Activity Feed */
.dashboard-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.8rem;
  margin-bottom: 2rem;
}

.notifications-section,
.activity-feed {
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 2rem;
  border: 1px solid rgba(0, 0, 0, 0.1);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.8rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
}

.section-title {
  color: #1e40af;
  font-size: 1.5rem;
  font-weight: 700;
  background: linear-gradient(45deg, #2563eb, #3b82f6);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.view-all-btn {
  background: rgba(37, 99, 235, 0.1);
  color: #2563eb;
  border: 1px solid rgba(37, 99, 235, 0.2);
  padding: 0.6rem 1.2rem;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 500;
}

.view-all-btn:hover {
  background: rgba(37, 99, 235, 0.2);
  transform: translateY(-2px);
}

/* Enhanced Notification Items */
.notification-item {
  display: flex;
  gap: 1.2rem;
  padding: 1.2rem;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(5px);
  border-radius: 12px;
  transition: all 0.3s ease;
  border: 1px solid rgba(0, 0, 0, 0.05);
}

.notification-item:hover {
  background: rgba(255, 255, 255, 0.95);
  transform: translateX(5px);
  border-color: rgba(37, 99, 235, 0.2);
}

.notification-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.4rem;
}

.notification-icon.urgent {
  background: rgba(255, 68, 68, 0.2);
  color: #ff4444;
}

.notification-icon.update {
  background: rgba(54, 162, 235, 0.2);
  color: #36a2eb;
}

.notification-icon.success {
  background: rgba(0, 255, 135, 0.2);
  color: #00ff87;
}

/* Enhanced Activity Items */
.activity-item {
  display: flex;
  gap: 1.2rem;
  padding: 1.2rem;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(5px);
  border-radius: 12px;
  transition: all 0.3s ease;
  border: 1px solid rgba(0, 0, 0, 0.05);
}

.activity-item:hover {
  background: rgba(255, 255, 255, 0.95);
  transform: translateX(5px);
  border-color: rgba(37, 99, 235, 0.2);
}

.activity-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: rgba(0, 255, 135, 0.1);
  color: #00ff87;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.4rem;
}

/* Enhanced Chart Section */
.page03-container18 {
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 2rem;
  margin-bottom: 2rem;
  border: 1px solid rgba(0, 0, 0, 0.1);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
}

.chart-legend {
  display: flex;
  gap: 1.2rem;
  flex-wrap: wrap;
  margin-top: 1rem;
  padding: 1rem;
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  border-radius: 12px;
  border: 1px solid rgba(0, 0, 0, 0.1);
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  padding: 0.4rem 0.8rem;
  background: rgba(255, 255, 255, 0.8);
  border-radius: 20px;
  border: 1px solid rgba(0, 0, 0, 0.05);
}

.legend-color {
  width: 12px;
  height: 12px;
  border-radius: 3px;
}

.legend-label {
  color: #4b5563;
  font-size: 0.9rem;
  font-weight: 500;
}

/* Enhanced Recent Complaints */
.page03-container20 {
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 2rem;
  border: 1px solid rgba(0, 0, 0, 0.1);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
}

.complaint-item {
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(5px);
  border-radius: 12px;
  padding: 1.5rem;
  transition: all 0.3s ease;
  border: 1px solid rgba(0, 0, 0, 0.05);
  margin-bottom: 1rem;
}

.complaint-item:hover {
  background: rgba(255, 255, 255, 0.95);
  transform: translateX(5px);
  border-color: rgba(37, 99, 235, 0.2);
}

.complaint-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.complaint-title {
  color: #1e40af;
  font-size: 1.2rem;
  font-weight: 600;
}

.complaint-status {
  padding: 0.4rem 1rem;
  border-radius: 20px;
  font-size: 0.9rem;
  font-weight: 500;
}

.complaint-description {
  color: #4b5563;
  font-size: 1rem;
  margin-bottom: 1rem;
  line-height: 1.6;
  font-weight: 400;
}

.complaint-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.9rem;
  color: rgba(255, 255, 255, 0.5);
}

.complaint-date,
.complaint-category {
  display: flex;
  align-items: center;
  gap: 0.6rem;
}

.complaint-category {
  background: rgba(255, 255, 255, 0.15);
  padding: 0.4rem 1rem;
  border-radius: 20px;
  color: #ffffff;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

/* Responsive Design */
@media (max-width: 1200px) {
  .dashboard-grid {
    grid-template-columns: 1fr;
  }
  
  .page03-container13 {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 768px) {
  .page03-container11 {
    width: 100%;
    height: auto;
    position: relative;
    z-index: 1;
  }
  
  .main-content {
    margin-left: 0;
    width: 100%;
    padding: 1rem;
  }
  
  .quick-actions {
  flex-direction: column;
  }
  
  .action-btn {
    width: 100%;
  }
  
  .dashboard-grid {
    grid-template-columns: 1fr;
  }
  
  .page03-container13,
  .page03-container18,
  .page03-container20 {
    margin-left: 0;
    width: 100%;
  }
  
  .section-header {
    flex-direction: column;
    gap: 1rem;
    align-items: flex-start;
  }
  
  .chart-legend {
    width: 100%;
    justify-content: flex-start;
  }
}

/* Animations */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.page03-container14,
.page03-container15,
.page03-container16,
.page03-container17,
.complaint-item,
.notification-item,
.activity-item {
  animation: fadeIn 0.5s ease-out forwards;
}

.page03-container14 { animation-delay: 0.1s; }
.page03-container15 { animation-delay: 0.2s; }
.page03-container16 { animation-delay: 0.3s; }
.page03-container17 { animation-delay: 0.4s; }

/* Enhanced Notifications and Activity Feed */
.notification-text {
  color: #1e293b;
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
  line-height: 1.5;
  font-weight: 500;
}

.notification-time {
  color: #64748b;
  font-size: 0.95rem;
  font-weight: 500;
}

.notification-icon.urgent {
  background: rgba(255, 68, 68, 0.2);
  color: #ff4444;
  border: 1px solid rgba(255, 68, 68, 0.3);
}

.notification-icon.update {
  background: rgba(54, 162, 235, 0.2);
  color: #36a2eb;
  border: 1px solid rgba(54, 162, 235, 0.3);
}

.notification-icon.success {
  background: rgba(0, 255, 135, 0.2);
  color: #00ff87;
  border: 1px solid rgba(0, 255, 135, 0.3);
}

/* Enhanced Activity Items */
.activity-text {
  color: #1e293b;
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
  line-height: 1.5;
  font-weight: 500;
}

.activity-time {
  color: #64748b;
  font-size: 0.95rem;
  font-weight: 500;
}

.activity-department {
  background: rgba(255, 255, 255, 0.15);
  padding: 0.4rem 1rem;
  border-radius: 20px;
  color: #ffffff;
  font-weight: 500;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

/* Enhanced Chart Section */
.page03-text25 {
  color: #ffffff;
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 1rem;
  display: block;
}

.chart-legend {
  display: flex;
  gap: 1.2rem;
  flex-wrap: wrap;
  margin-top: 1rem;
  padding: 1rem;
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  border-radius: 12px;
  border: 1px solid rgba(0, 0, 0, 0.1);
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  padding: 0.4rem 0.8rem;
  background: rgba(255, 255, 255, 0.8);
  border-radius: 20px;
  border: 1px solid rgba(0, 0, 0, 0.05);
}

.legend-color {
  width: 12px;
  height: 12px;
  border-radius: 3px;
}

.legend-label {
  color: #4b5563;
  font-size: 0.9rem;
  font-weight: 500;
}

/* Enhanced Recent Complaints */
.complaint-title {
  color: #1e40af;
  font-size: 1.2rem;
  font-weight: 600;
}

.complaint-description {
  color: #4b5563;
  font-size: 1.1rem;
  margin-bottom: 1rem;
  line-height: 1.6;
  font-weight: 400;
}

.complaint-status {
  padding: 0.4rem 1rem;
  border-radius: 20px;
  font-size: 0.95rem;
  font-weight: 600;
  text-transform: capitalize;
}

.complaint-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.95rem;
  color: rgba(255, 255, 255, 0.8);
  font-weight: 500;
}

.complaint-date,
.complaint-category {
  display: flex;
  align-items: center;
  gap: 0.6rem;
}

.complaint-category {
  background: rgba(255, 255, 255, 0.15);
  padding: 0.4rem 1rem;
  border-radius: 20px;
  color: #ffffff;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

/* Section Headers */
.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.8rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
}

.section-title {
  color: #1e40af;
  font-size: 1.5rem;
  font-weight: 700;
  background: linear-gradient(45deg, #2563eb, #3b82f6);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 0 0 30px rgba(37, 99, 235, 0.2);
}

/* Enhanced Card Backgrounds */
.notifications-section,
.activity-feed,
.page03-container18,
  .page03-container20 {
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 2rem;
  border: 1px solid rgba(0, 0, 0, 0.1);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
}

.notification-item,
.activity-item,
.complaint-item {
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(5px);
  border-radius: 12px;
  padding: 1.5rem;
  transition: all 0.3s ease;
  border: 1px solid rgba(0, 0, 0, 0.05);
  margin-bottom: 1rem;
}

.notification-item:hover,
.activity-item:hover,
.complaint-item:hover {
  background: rgba(255, 255, 255, 0.95);
  transform: translateX(5px);
  border-color: rgba(37, 99, 235, 0.2);
}

/* Enhanced KPI Cards with Emergency Colors */
.page03-container13 {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.page03-container14,
.page03-container15,
.page03-container16,
.page03-container17 {
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 2rem;
  border: 1px solid rgba(0, 0, 0, 0.1);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 1.2rem;
  position: relative;
  overflow: hidden;
}

/* Emergency Status Indicators */
.page03-container14::before,
.page03-container15::before,
.page03-container16::before,
.page03-container17::before {
  content: '';
    position: absolute;
  top: 0;
  left: 0;
  width: 4px;
  height: 100%;
  background: #00ff87;
  transition: all 0.3s ease;
}

.page03-container14.emergency::before { background: #ff4444; }
.page03-container15.emergency::before { background: #ff4444; }
.page03-container16.emergency::before { background: #ff4444; }
.page03-container17.emergency::before { background: #ff4444; }

.page03-container14.warning::before { background: #ffbb33; }
.page03-container15.warning::before { background: #ffbb33; }
.page03-container16.warning::before { background: #ffbb33; }
.page03-container17.warning::before { background: #ffbb33; }

.stat-icon {
  width: 56px;
  height: 56px;
  border-radius: 14px;
  background: rgba(0, 255, 135, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.8rem;
  color: #00ff87;
  transition: all 0.3s ease;
}

.emergency .stat-icon {
  background: rgba(255, 68, 68, 0.1);
  color: #ff4444;
}

.warning .stat-icon {
  background: rgba(255, 187, 51, 0.1);
  color: #ffbb33;
}

.stat-content {
  flex: 1;
}

.page03-text13,
.page03-text16,
.page03-text20,
.page03-text22 {
  color: #1e40af;
  font-size: 1.3rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
  display: block;
}

.page03-text14,
.page03-text18,
.page03-text19,
.page03-text23 {
  color: #4b5563;
  font-size: 1rem;
  margin-bottom: 0.3rem;
  display: block;
}

.page03-text15,
.page03-text17,
.page03-text21,
.page03-text24 {
  color: #2563eb;
  font-size: 1.1rem;
  font-weight: 500;
  display: block;
}

.emergency .page03-text15,
.emergency .page03-text17,
.emergency .page03-text21,
.emergency .page03-text24 {
  color: #ff4444;
}

.warning .page03-text15,
.warning .page03-text17,
.warning .page03-text21,
.warning .page03-text24 {
  color: #ffbb33;
}

/* Timeline Section Styles */
.timeline-section {
  background: #ffffff;
  border-radius: 16px;
  padding: 2rem;
  margin-top: 2rem;
  border: 2px solid rgba(37, 99, 235, 0.2);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
}

.timeline-container {
  position: relative;
  padding: 1rem 0;
}

.timeline-container::before {
  content: '';
  position: absolute;
  left: 120px;
  top: 0;
  bottom: 0;
  width: 2px;
  background: rgba(37, 99, 235, 0.2);
}

.timeline-item {
  display: flex;
  margin-bottom: 2rem;
  position: relative;
  transition: all 0.3s ease;
}

.timeline-item.completed {
  opacity: 0.7;
}

.timeline-item.completed .timeline-content {
  background: rgba(37, 99, 235, 0.05);
}

.timeline-time {
  width: 120px;
  padding-right: 2rem;
  text-align: right;
  position: relative;
}

.timeline-time .time {
  font-size: 0.9rem;
  color: #4b5563;
  font-weight: 500;
}

.timeline-dot {
  position: absolute;
  right: -6px;
  top: 50%;
  transform: translateY(-50%);
  width: 12px;
  height: 12px;
  border-radius: 50%;
  background: #2563eb;
  border: 2px solid #ffffff;
  box-shadow: 0 0 0 2px rgba(37, 99, 235, 0.2);
}

.timeline-item.completed .timeline-dot {
  background: #10b981;
}

.timeline-content {
  flex: 1;
  background: #ffffff;
  border-radius: 12px;
  padding: 1.5rem;
  margin-left: 2rem;
  border: 1px solid rgba(37, 99, 235, 0.2);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
}

.timeline-content:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.timeline-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.8rem;
}

.timeline-header h4 {
  font-size: 1.1rem;
  color: #1e40af;
  margin: 0;
}

.event-status {
  padding: 0.4rem 1rem;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 500;
}

.event-status.high {
  background: rgba(239, 68, 68, 0.1);
  color: #dc2626;
  border: 1px solid rgba(239, 68, 68, 0.2);
}

.event-status.medium {
  background: rgba(234, 179, 8, 0.1);
  color: #b45309;
  border: 1px solid rgba(234, 179, 8, 0.2);
}

.event-status.low {
  background: rgba(34, 197, 94, 0.1);
  color: #15803d;
  border: 1px solid rgba(34, 197, 94, 0.2);
}

.event-description {
  color: #4b5563;
  font-size: 0.95rem;
  margin-bottom: 1rem;
  line-height: 1.5;
}

.event-meta {
  display: flex;
  gap: 1.5rem;
  margin-bottom: 1rem;
  font-size: 0.9rem;
  color: #6b7280;
}

.event-location,
.event-duration {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.event-actions {
  display: flex;
  gap: 1rem;
}

.event-actions .action-btn {
  padding: 0.5rem 1rem;
  border: 1px solid rgba(37, 99, 235, 0.2);
  border-radius: 6px;
  background: #ffffff;
  color: #2563eb;
  font-size: 0.9rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  transition: all 0.3s ease;
}

.event-actions .action-btn:hover {
  background: rgba(37, 99, 235, 0.05);
  transform: translateY(-1px);
}

/* Responsive Design for Timeline */
@media (max-width: 768px) {
  .timeline-container::before {
    left: 80px;
  }

  .timeline-time {
    width: 80px;
  }

  .timeline-content {
    margin-left: 1rem;
  }

  .event-meta {
    flex-direction: column;
    gap: 0.5rem;
  }

  .event-actions {
    flex-direction: column;
  }

  .event-actions .action-btn {
    width: 100%;
    justify-content: center;
  }
}
</style>
