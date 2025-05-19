<template>
  <div class="modal-overlay" @click="$emit('close')">
    <div class="modal-content" @click.stop>
      <div class="modal-header">
        <h2>{{ project ? 'Edit Project' : 'Add New Project' }}</h2>
        <button class="close-button" @click="$emit('close')">
          <i class="fas fa-times"></i>
        </button>
      </div>

      <form @submit.prevent="handleSubmit" class="project-form">
        <div class="form-grid">
          <div class="form-group">
            <label for="title">Project Title</label>
            <input 
              type="text" 
              id="title" 
              v-model="formData.title" 
              required
              placeholder="Enter project title"
            >
          </div>

          <div class="form-group">
            <label for="department">Department</label>
            <select id="department" v-model="formData.department" required>
              <option value="">Select Department</option>
              <option v-for="dept in departments" :key="dept" :value="dept">
                {{ dept }}
              </option>
            </select>
          </div>

          <div class="form-group">
            <label for="status">Status</label>
            <select id="status" v-model="formData.status" required>
              <option value="planned">Planned</option>
              <option value="in-progress">In Progress</option>
              <option value="completed">Completed</option>
            </select>
          </div>

          <div class="form-group">
            <label for="start_date">Start Date</label>
            <input 
              type="date" 
              id="start_date" 
              v-model="formData.start_date" 
              required
            >
          </div>

          <div class="form-group">
            <label for="end_date">End Date</label>
            <input 
              type="date" 
              id="end_date" 
              v-model="formData.end_date" 
              required
            >
          </div>

          <div class="form-group">
            <label for="budget">Budget (₹)</label>
            <input 
              type="number" 
              id="budget" 
              v-model="formData.budget" 
              placeholder="Enter budget amount"
            >
          </div>
        </div>

        <div class="form-group full-width">
          <label for="description">Description</label>
          <textarea 
            id="description" 
            v-model="formData.description" 
            rows="4" 
            required
            placeholder="Enter project description"
          ></textarea>
        </div>

        <div class="form-group full-width">
          <label for="engineers">Assigned Engineers</label>
          <div class="engineers-list">
            <div v-for="(engineer, index) in formData.assigned_engineers" :key="index" class="engineer-tag">
              {{ engineer }}
              <button type="button" @click="removeEngineer(index)" class="remove-engineer">
                <i class="fas fa-times"></i>
              </button>
            </div>
            <input 
              type="text" 
              v-model="newEngineer" 
              @keydown.enter.prevent="addEngineer"
              placeholder="Add engineer and press Enter"
            >
          </div>
        </div>

        <div class="form-group full-width">
          <label>Documents</label>
          <div class="file-upload">
            <input 
              type="file" 
              @change="handleFileUpload" 
              multiple
              accept=".pdf,.doc,.docx,.xls,.xlsx"
            >
            <div class="uploaded-files">
              <div v-for="(file, index) in formData.documents" :key="index" class="file-tag">
                {{ file.name }}
                <button type="button" @click="removeFile(index)" class="remove-file">
                  <i class="fas fa-times"></i>
                </button>
              </div>
            </div>
          </div>
        </div>

        <div class="form-actions">
          <button type="button" class="cancel-button" @click="$emit('close')">Cancel</button>
          <button type="submit" class="save-button">Save Project</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ProjectModal',
  props: {
    project: {
      type: Object,
      default: null
    }
  },
  data() {
    return {
      formData: {
        title: '',
        department: '',
        status: 'planned',
        start_date: '',
        end_date: '',
        budget: null,
        description: '',
        assigned_engineers: [],
        documents: []
      },
      newEngineer: '',
      departments: [
        'Water Supply',
        'Roads & Bridges',
        'Sanitation',
        'Electricity',
        'Public Works'
      ]
    }
  },
  watch: {
    project: {
      immediate: true,
      handler(newProject) {
        if (newProject) {
          this.formData = { ...newProject }
        }
      }
    }
  },
  methods: {
    handleSubmit() {
      this.$emit('save', this.formData)
    },
    addEngineer() {
      if (this.newEngineer.trim()) {
        this.formData.assigned_engineers.push(this.newEngineer.trim())
        this.newEngineer = ''
      }
    },
    removeEngineer(index) {
      this.formData.assigned_engineers.splice(index, 1)
    },
    handleFileUpload(event) {
      const files = Array.from(event.target.files)
      files.forEach(file => {
        this.formData.documents.push({
          name: file.name,
          file: file
        })
      })
    },
    removeFile(index) {
      this.formData.documents.splice(index, 1)
    }
  }
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background: #ffffff;
  border-radius: 12px;
  width: 90%;
  max-width: 800px;
  max-height: 90vh;
  overflow-y: auto;
  padding: 2rem;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
  padding-bottom: 1rem;
  border-bottom: 2px solid rgba(37, 99, 235, 0.1);
}

.modal-header h2 {
  color: #1e40af;
  font-size: 1.5rem;
  font-weight: 600;
}

.close-button {
  background: none;
  border: none;
  color: #4b5563;
  font-size: 1.2rem;
  cursor: pointer;
  padding: 0.5rem;
  border-radius: 4px;
  transition: all 0.3s ease;
}

.close-button:hover {
  background: rgba(37, 99, 235, 0.1);
  color: #2563eb;
}

.project-form {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.form-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-group.full-width {
  grid-column: 1 / -1;
}

label {
  color: #4b5563;
  font-size: 0.9rem;
  font-weight: 500;
}

input,
select,
textarea {
  padding: 0.6rem 1rem;
  border: 1px solid rgba(37, 99, 235, 0.2);
  border-radius: 8px;
  font-size: 0.9rem;
  transition: all 0.3s ease;
}

input:focus,
select:focus,
textarea:focus {
  outline: none;
  border-color: #2563eb;
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.1);
}

.engineers-list {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  padding: 0.5rem;
  border: 1px solid rgba(37, 99, 235, 0.2);
  border-radius: 8px;
}

.engineer-tag {
  background: rgba(37, 99, 235, 0.1);
  color: #2563eb;
  padding: 0.3rem 0.8rem;
  border-radius: 20px;
  font-size: 0.9rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.remove-engineer {
  background: none;
  border: none;
  color: #2563eb;
  cursor: pointer;
  padding: 0.2rem;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.remove-engineer:hover {
  background: rgba(37, 99, 235, 0.2);
}

.file-upload {
  border: 2px dashed rgba(37, 99, 235, 0.2);
  border-radius: 8px;
  padding: 1rem;
}

.uploaded-files {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-top: 1rem;
}

.file-tag {
  background: rgba(37, 99, 235, 0.1);
  color: #2563eb;
  padding: 0.3rem 0.8rem;
  border-radius: 20px;
  font-size: 0.9rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.remove-file {
  background: none;
  border: none;
  color: #2563eb;
  cursor: pointer;
  padding: 0.2rem;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.remove-file:hover {
  background: rgba(37, 99, 235, 0.2);
}

.form-actions {
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
  margin-top: 2rem;
  padding-top: 1rem;
  border-top: 2px solid rgba(37, 99, 235, 0.1);
}

.cancel-button,
.save-button {
  padding: 0.6rem 1.2rem;
  border-radius: 8px;
  font-size: 0.9rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
}

.cancel-button {
  background: none;
  border: 1px solid rgba(37, 99, 235, 0.2);
  color: #4b5563;
}

.cancel-button:hover {
  background: rgba(37, 99, 235, 0.05);
  border-color: rgba(37, 99, 235, 0.3);
}

.save-button {
  background: #2563eb;
  border: none;
  color: #ffffff;
}

.save-button:hover {
  background: #1d4ed8;
}
</style> 