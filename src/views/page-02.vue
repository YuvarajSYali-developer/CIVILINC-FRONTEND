<template>
  <div class="page02-container">
    <div class="fluid-background">
      <div class="fluid-effect"></div>
      <div class="gradient-overlay"></div>
    </div>
    <div class="sign-in-wrapper">
      <div class="sign-in-left">
        <div class="brand-section">
          <span class="brand-text"><span class="code-brackets">&lt;/&gt;</span> Civilinc</span>
        </div>
        <div class="image-overlay"></div>
        <img
          src="https://images.unsplash.com/photo-1486406146926-c627a92ad1ab?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80"
          alt="Smart City"
          class="background-image"
        />
      </div>
      <div class="sign-in-right">
        <div class="sign-in-content">
          <router-link to="/" class="back-to-home">
            <svg viewBox="0 0 24 24" class="back-icon">
              <path d="M20 11H7.83l5.59-5.59L12 4l-8 8 8 8 1.41-1.41L7.83 13H20v-2z"/>
            </svg>
            Back to Home
          </router-link>
          <h1 class="sign-in-title">Welcome Back</h1>
          <p class="sign-in-subtitle">Sign in to continue to Civilinc</p>
          
          <form @submit.prevent="handleSignIn" class="sign-in-form">
            <div class="form-group">
              <label for="email">Email</label>
              <input
                type="email"
                id="email"
                v-model="email"
                @blur="validateEmail"
                :class="{ error: emailError }"
                placeholder="Enter your email"
                required
              />
              <span v-if="emailError" class="error-message">{{ emailError }}</span>
            </div>
            
            <div class="form-group">
              <label for="password">Password</label>
              <div class="password-input">
                <input
                  :type="showPassword ? 'text' : 'password'"
                  id="password"
                  v-model="password"
                  @blur="validatePassword"
                  :class="{ error: passwordError }"
                  placeholder="Enter your password"
                  required
                />
                <button
                  type="button"
                  class="toggle-password"
                  @click="togglePassword"
                  :aria-label="showPassword ? 'Hide password' : 'Show password'"
                >
                  <i :class="showPassword ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
                </button>
              </div>
              <span v-if="passwordError" class="error-message">{{ passwordError }}</span>
            </div>
            
            <div class="form-options">
              <label class="remember-me">
                <input type="checkbox" v-model="rememberMe" />
                <span>Remember me</span>
              </label>
              <button type="button" class="forgot-password" @click="handleForgotPassword">
                Forgot password?
              </button>
            </div>
            
            <button
              type="submit"
              class="sign-in-button"
              :disabled="!isFormValid || isLoading"
              :class="{ loading: isLoading }"
            >
              <span v-if="isLoading">Signing in...</span>
              <span v-else>Sign In</span>
            </button>
          </form>
          
          <div class="divider">
            <span>or continue with</span>
          </div>
          
          <div class="social-login">
            <button 
              class="social-button google" 
              @click="handleGoogleSignIn"
              :disabled="isLoading"
            >
              <svg viewBox="0 0 24 24" class="social-icon">
                <path d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z" fill="#4285F4"/>
                <path d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z" fill="#34A853"/>
                <path d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z" fill="#FBBC05"/>
                <path d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z" fill="#EA4335"/>
              </svg>
              Google
            </button>
            <button 
              class="social-button facebook" 
              @click="handleFacebookSignIn"
              :disabled="isLoading"
            >
              <svg viewBox="0 0 24 24" class="social-icon">
                <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
              </svg>
              Facebook
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SignIn',
  data() {
    return {
      email: '',
      password: '',
      rememberMe: false,
      showPassword: false,
      isLoading: false,
      emailError: '',
      passwordError: ''
    }
  },
  computed: {
    isFormValid() {
      return this.email && this.password && !this.emailError && !this.passwordError
    }
  },
  methods: {
    validateEmail() {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
      if (!this.email) {
        this.emailError = 'Email is required'
      } else if (!emailRegex.test(this.email)) {
        this.emailError = 'Please enter a valid email address'
      } else {
        this.emailError = ''
      }
    },
    validatePassword() {
      if (!this.password) {
        this.passwordError = 'Password is required'
      } else if (this.password.length < 6) {
        this.passwordError = 'Password must be at least 6 characters'
      } else {
        this.passwordError = ''
      }
    },
    togglePassword() {
      this.showPassword = !this.showPassword
    },
    handleSignIn() {
      // Validate form
      this.validateEmail()
      this.validatePassword()
      
      if (!this.isFormValid) {
        return
      }

      // Show loading state
      this.isLoading = true

      // Check for specific credentials
      if (this.email === 'yuvarajyali@gmail.com' && this.password === 'yuvarajyali@gmail.com') {
        // Store user session
        localStorage.setItem('isAuthenticated', 'true')
        localStorage.setItem('user', JSON.stringify({
          email: this.email,
          name: 'Yuvaraj Yali'
        }))

        // Navigate to dashboard
        this.$router.push('/dashboard')
      } else {
        // Show error for invalid credentials
        this.emailError = 'Invalid email or password'
        this.passwordError = 'Invalid email or password'
      }
      
      // Reset loading state
      this.isLoading = false
    },
    handleGoogleSignIn() {
      // Implement Google sign-in
      console.log('Google sign-in clicked')
    },
    handleFacebookSignIn() {
      // Implement Facebook sign-in
      console.log('Facebook sign-in clicked')
    },
    handleForgotPassword() {
      // Implement forgot password functionality
      console.log('Forgot password clicked')
    }
  }
}
</script>

<style scoped>
.page02-container {
  width: 100%;
  min-height: 100vh;
  background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  overflow: hidden;
}

.fluid-background {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  overflow: hidden;
}

.fluid-effect {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 150%;
  height: 150%;
  background: radial-gradient(
    circle at center,
    rgba(0, 255, 135, 0.12) 0%,
    rgba(96, 239, 255, 0.08) 25%,
    rgba(0, 255, 135, 0.04) 50%,
    rgba(96, 239, 255, 0.08) 75%,
    rgba(0, 255, 135, 0.12) 100%
  );
  animation: fluidDistortion 25s infinite linear;
  pointer-events: none;
  z-index: 0;
  filter: blur(30px);
  opacity: 0.8;
}

.gradient-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(
    135deg,
    rgba(15, 23, 42, 0.98) 0%,
    rgba(30, 41, 59, 0.96) 25%,
    rgba(51, 65, 85, 0.94) 50%,
    rgba(30, 41, 59, 0.96) 75%,
    rgba(15, 23, 42, 0.98) 100%
  );
  z-index: 1;
  backdrop-filter: blur(10px);
  box-shadow: 
    inset 0 0 150px rgba(0, 0, 0, 0.4),
    inset 0 0 50px rgba(0, 255, 135, 0.05);
}

.sign-in-wrapper {
  width: 1000px;
  height: 600px;
  background: rgba(255, 255, 255, 0.02);
  border-radius: 24px;
  backdrop-filter: blur(25px);
  display: flex;
  overflow: hidden;
  box-shadow: 
    0 20px 40px rgba(0, 0, 0, 0.3),
    0 0 0 1px rgba(255, 255, 255, 0.03),
    inset 0 0 0 1px rgba(255, 255, 255, 0.03);
  position: relative;
  z-index: 2;
  animation: fadeIn 0.6s cubic-bezier(0.4, 0, 0.2, 1) forwards;
}

.sign-in-left {
  width: 50%;
  position: relative;
  overflow: hidden;
}

.brand-section {
  position: absolute;
  top: 2rem;
  left: 2rem;
  z-index: 2;
  animation: slideDown 0.6s cubic-bezier(0.4, 0, 0.2, 1) forwards;
}

.brand-text {
  font-size: 1.8rem;
  font-weight: 700;
  background: linear-gradient(45deg, #00ff87, #60efff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 0 0 30px rgba(0, 255, 135, 0.3);
  display: flex;
  align-items: center;
  gap: 0.5rem;
  letter-spacing: 0.5px;
}

.code-brackets {
  font-family: 'Fira Code', monospace;
  font-size: 1.6rem;
  opacity: 0.8;
}

.background-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  position: absolute;
  top: 0;
  left: 0;
  transform: scale(1.1);
  transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1);
  filter: brightness(0.75) contrast(1.15) saturate(1.1);
}

.sign-in-left:hover .background-image {
  transform: scale(1);
}

.image-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(
    45deg,
    rgba(0, 0, 0, 0.85) 0%,
    rgba(0, 0, 0, 0.45) 50%,
    rgba(0, 0, 0, 0.85) 100%
  );
  z-index: 1;
  mix-blend-mode: multiply;
}

.sign-in-right {
  width: 50%;
  padding: 3.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(255, 255, 255, 0.01);
}

.sign-in-content {
  width: 100%;
  max-width: 400px;
  animation: slideIn 0.6s cubic-bezier(0.4, 0, 0.2, 1) forwards;
}

.sign-in-title {
  font-size: 2.5rem;
  font-weight: 700;
  color: #ffffff;
  margin-bottom: 0.5rem;
  background: linear-gradient(45deg, #ffffff, #60efff);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  letter-spacing: -0.5px;
}

.sign-in-subtitle {
  color: rgba(255, 255, 255, 0.7);
  margin-bottom: 2rem;
}

.form-group {
  margin-bottom: 1.5rem;
  animation: fadeIn 0.5s ease-out forwards;
  animation-delay: calc(var(--i) * 0.1s);
}

.form-group label {
  display: block;
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 0.5rem;
  font-size: 0.9rem;
}

.form-input {
  width: 100%;
  padding: 1rem 1.2rem;
  background: rgba(255, 255, 255, 0.03);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 12px;
  color: #ffffff;
  font-size: 1rem;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.form-input:focus {
  outline: none;
  border-color: #00ff87;
  box-shadow: 0 0 0 3px rgba(0, 255, 135, 0.1);
  background: rgba(255, 255, 255, 0.05);
}

.password-input-wrapper {
  position: relative;
}

.toggle-password {
  position: absolute;
  right: 1rem;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  color: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  padding: 0;
  transition: color 0.3s ease;
}

.toggle-password:hover {
  color: #00ff87;
}

.eye-icon {
  width: 20px;
  height: 20px;
  fill: currentColor;
}

.form-options {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.5rem;
}

.remember-me {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9rem;
}

.forgot-password {
  color: #00ff87;
  text-decoration: none;
  font-size: 0.9rem;
  transition: color 0.3s ease;
}

.forgot-password:hover {
  color: #60efff;
}

.sign-in-button {
  width: 100%;
  padding: 1.1rem;
  background: linear-gradient(45deg, #00ff87, #60efff);
  border: none;
  border-radius: 12px;
  color: #000;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.sign-in-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.2),
    transparent
  );
  transition: 0.5s;
}

.sign-in-button:hover::before {
  left: 100%;
}

.sign-in-button:hover {
  transform: translateY(-2px);
  box-shadow: 
    0 10px 20px rgba(0, 255, 135, 0.2),
    0 0 0 1px rgba(0, 255, 135, 0.1);
}

.divider {
  text-align: center;
  margin: 2rem 0;
  position: relative;
}

.divider::before,
.divider::after {
  content: '';
  position: absolute;
  top: 50%;
  width: calc(50% - 70px);
  height: 1px;
  background: rgba(255, 255, 255, 0.1);
}

.divider::before {
  left: 0;
}

.divider::after {
  right: 0;
}

.divider span {
  background: rgba(255, 255, 255, 0.05);
  padding: 0 1rem;
  color: rgba(255, 255, 255, 0.5);
  font-size: 0.9rem;
}

.social-login {
  display: flex;
  gap: 1rem;
}

.social-button {
  flex: 1;
  padding: 0.8rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  background: rgba(255, 255, 255, 0.05);
  color: #ffffff;
  font-size: 0.9rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.social-button:hover {
  background: rgba(255, 255, 255, 0.1);
  transform: translateY(-2px);
}

.social-icon {
  width: 20px;
  height: 20px;
}

.back-to-home {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: rgba(255, 255, 255, 0.7);
  text-decoration: none;
  font-size: 0.9rem;
  margin-bottom: 2rem;
  transition: color 0.3s ease;
}

.back-to-home:hover {
  color: #00ff87;
}

.back-icon {
  width: 20px;
  height: 20px;
  fill: currentColor;
}

/* Animations */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: scale(0.98);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateX(40px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes fluidDistortion {
  0% {
    transform: translate(-50%, -50%) rotate(0deg) scale(1);
  }
  50% {
    transform: translate(-50%, -50%) rotate(180deg) scale(1.15);
  }
  100% {
    transform: translate(-50%, -50%) rotate(360deg) scale(1);
  }
}

/* Responsive Design */
@media (max-width: 1024px) {
  .sign-in-wrapper {
    width: 90%;
    max-width: 1000px;
  }
}

@media (max-width: 768px) {
  .sign-in-wrapper {
    flex-direction: column;
    height: auto;
    min-height: 100vh;
  }
  
  .sign-in-left,
  .sign-in-right {
    width: 100%;
  }
  
  .sign-in-left {
    height: 200px;
  }
  
  .sign-in-right {
    padding: 2rem;
  }
  
  .social-login {
    flex-direction: column;
  }
  
  .social-button {
    width: 100%;
  }
}

@media (max-width: 480px) {
  .sign-in-content {
    padding: 1rem;
  }
  
  .form-options {
    flex-direction: column;
    gap: 1rem;
    align-items: flex-start;
  }
  
  .sign-in-title {
    font-size: 2rem;
  }
}

/* Add error styles */
.error-message {
  color: #ff4d4f;
  font-size: 0.8rem;
  margin-top: 0.25rem;
}

.form-input.error {
  border-color: #ff4d4f;
}

.form-input.error:focus {
  box-shadow: 0 0 0 3px rgba(255, 77, 79, 0.1);
}

/* Add loading state styles */
.sign-in-button.loading {
  opacity: 0.7;
  cursor: not-allowed;
}

.sign-in-button:disabled {
  opacity: 0.7;
  cursor: not-allowed;
  transform: none;
}

.sign-in-button:disabled:hover {
  transform: none;
  box-shadow: none;
}

.social-button:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.social-button:disabled:hover {
  transform: none;
  background: rgba(255, 255, 255, 0.05);
}
</style>
