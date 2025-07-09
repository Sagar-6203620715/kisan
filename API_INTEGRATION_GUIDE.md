# Project Kisan - API Integration Guide

## 1. Google AI APIs (Core Technologies)

### Vertex AI - Gemini Pro Vision
```javascript
// Disease Diagnosis API Integration
const geminiVisionAPI = {
  endpoint: 'https://generativelanguage.googleapis.com/v1beta/models/gemini-pro-vision:generateContent',
  
  async analyzeCropImage(imageBase64, prompt) {
    const requestBody = {
      contents: [{
        parts: [
          {
            text: prompt
          },
          {
            inline_data: {
              mime_type: "image/jpeg",
              data: imageBase64
            }
          }
        ]
      }],
      generationConfig: {
        temperature: 0.1,
        topK: 32,
        topP: 1,
        maxOutputTokens: 2048,
      }
    };
    
    const response = await fetch(`${this.endpoint}?key=${API_KEY}`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(requestBody)
    });
    
    return response.json();
  }
};
```

### Vertex AI - Gemini Pro
```javascript
// Text Analysis for Market Intelligence
const geminiProAPI = {
  endpoint: 'https://generativelanguage.googleapis.com/v1beta/models/gemini-pro:generateContent',
  
  async analyzeMarketData(marketData, query) {
    const prompt = `
      Analyze the following market data for agricultural commodities:
      ${JSON.stringify(marketData)}
      
      Query: ${query}
      
      Provide:
      1. Price trend analysis
      2. Best selling recommendations
      3. Risk assessment
      4. Market predictions
      
      Response in Hindi/Kannada/Tamil based on user preference.
    `;
    
    const requestBody = {
      contents: [{
        parts: [{
          text: prompt
        }]
      }],
      generationConfig: {
        temperature: 0.2,
        maxOutputTokens: 1024,
      }
    };
    
    const response = await fetch(`${this.endpoint}?key=${API_KEY}`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(requestBody)
    });
    
    return response.json();
  }
};
```

### Vertex AI Speech-to-Text
```javascript
// Voice Input Processing
const speechToTextAPI = {
  endpoint: 'https://speech.googleapis.com/v1/speech:recognize',
  
  async convertSpeechToText(audioBlob, languageCode) {
    const audioContent = await this.blobToBase64(audioBlob);
    
    const requestBody = {
      config: {
        encoding: 'WEBM_OPUS',
        sampleRateHertz: 48000,
        languageCode: languageCode, // 'hi-IN', 'kn-IN', 'ta-IN', etc.
        model: 'latest_long',
        useEnhanced: true,
        enableAutomaticPunctuation: true,
        enableWordTimeOffsets: false,
        enableWordConfidence: true,
      },
      audio: {
        content: audioContent
      }
    };
    
    const response = await fetch(`${this.endpoint}?key=${API_KEY}`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(requestBody)
    });
    
    return response.json();
  }
};
```

### Vertex AI Text-to-Speech
```javascript
// Voice Output Generation
const textToSpeechAPI = {
  endpoint: 'https://texttospeech.googleapis.com/v1/text:synthesize',
  
  async convertTextToSpeech(text, languageCode, voiceName) {
    const requestBody = {
      input: {
        text: text
      },
      voice: {
        languageCode: languageCode,
        name: voiceName, // 'hi-IN-Standard-A', 'kn-IN-Standard-A'
        ssmlGender: 'FEMALE'
      },
      audioConfig: {
        audioEncoding: 'MP3',
        speakingRate: 0.9,
        pitch: 0,
        volumeGainDb: 0,
        effectsProfileId: ['headphone-class-device']
      }
    };
    
    const response = await fetch(`${this.endpoint}?key=${API_KEY}`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(requestBody)
    });
    
    return response.json();
  }
};
```

---

## 2. Market Data APIs

### Agmarknet API (Government Market Data)
```javascript
// Real-time Agricultural Market Prices
const agmarknetAPI = {
  baseUrl: 'https://agmarknet.gov.in/api/v1',
  
  async getMarketPrices(commodity, state, mandi) {
    const endpoint = `${this.baseUrl}/prices`;
    const params = new URLSearchParams({
      commodity: commodity,
      state: state,
      mandi: mandi,
      date: new Date().toISOString().split('T')[0]
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  },
  
  async getPriceHistory(commodity, mandi, days = 30) {
    const endpoint = `${this.baseUrl}/price-history`;
    const params = new URLSearchParams({
      commodity: commodity,
      mandi: mandi,
      days: days
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  },
  
  async getNearbyMandiPrices(latitude, longitude, radius = 50) {
    const endpoint = `${this.baseUrl}/nearby-mandis`;
    const params = new URLSearchParams({
      lat: latitude,
      lng: longitude,
      radius: radius
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  }
};
```

### NCDEX API (Commodity Exchange Data)
```javascript
// Commodity Trading and Futures Data
const ncdexAPI = {
  baseUrl: 'https://www.ncdex.com/api/v1',
  
  async getFuturesData(commodity) {
    const endpoint = `${this.baseUrl}/futures/${commodity}`;
    const response = await fetch(endpoint);
    return response.json();
  },
  
  async getSpotPrices(commodity) {
    const endpoint = `${this.baseUrl}/spot-prices/${commodity}`;
    const response = await fetch(endpoint);
    return response.json();
  },
  
  async getMarketSentiment(commodity) {
    const endpoint = `${this.baseUrl}/sentiment/${commodity}`;
    const response = await fetch(endpoint);
    return response.json();
  }
};
```

### Weather APIs for Agricultural Forecasting
```javascript
// Weather Data for Crop Planning
const weatherAPI = {
  baseUrl: 'https://api.openweathermap.org/data/2.5',
  
  async getWeatherForecast(latitude, longitude) {
    const endpoint = `${this.baseUrl}/forecast`;
    const params = new URLSearchParams({
      lat: latitude,
      lon: longitude,
      appid: WEATHER_API_KEY,
      units: 'metric'
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  },
  
  async getAgriculturalWeather(latitude, longitude) {
    const endpoint = `${this.baseUrl}/agricultural-weather`;
    const params = new URLSearchParams({
      lat: latitude,
      lon: longitude,
      appid: WEATHER_API_KEY
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  }
};
```

---

## 3. Government Scheme APIs

### PM-KISAN API
```javascript
// PM-KISAN Scheme Data
const pmKisanAPI = {
  baseUrl: 'https://pmkisan.gov.in/api/v1',
  
  async checkEligibility(aadharNumber) {
    const endpoint = `${this.baseUrl}/eligibility-check`;
    const requestBody = {
      aadhar_number: aadharNumber
    };
    
    const response = await fetch(endpoint, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(requestBody)
    });
    
    return response.json();
  },
  
  async getPaymentStatus(aadharNumber) {
    const endpoint = `${this.baseUrl}/payment-status`;
    const params = new URLSearchParams({
      aadhar: aadharNumber
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  },
  
  async getSchemeDetails() {
    const endpoint = `${this.baseUrl}/scheme-details`;
    const response = await fetch(endpoint);
    return response.json();
  }
};
```

### Farmer.gov.in API
```javascript
// Government Agricultural Schemes
const farmerGovAPI = {
  baseUrl: 'https://farmer.gov.in/api/v1',
  
  async getSchemesByCategory(category) {
    const endpoint = `${this.baseUrl}/schemes`;
    const params = new URLSearchParams({
      category: category // 'subsidy', 'loan', 'insurance', 'technology'
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  },
  
  async getSchemeDetails(schemeId) {
    const endpoint = `${this.baseUrl}/schemes/${schemeId}`;
    const response = await fetch(endpoint);
    return response.json();
  },
  
  async checkEligibility(schemeId, farmerProfile) {
    const endpoint = `${this.baseUrl}/schemes/${schemeId}/eligibility`;
    const requestBody = {
      farmer_profile: farmerProfile
    };
    
    const response = await fetch(endpoint, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(requestBody)
    });
    
    return response.json();
  }
};
```

---

## 4. Satellite and Land Analysis APIs

### Google Earth Engine API
```javascript
// Satellite Imagery for Land Analysis
const earthEngineAPI = {
  baseUrl: 'https://earthengine.googleapis.com/v1alpha',
  
  async getSatelliteImagery(latitude, longitude, date) {
    const endpoint = `${this.baseUrl}/images`;
    const params = new URLSearchParams({
      lat: latitude,
      lng: longitude,
      date: date,
      dataset: 'LANDSAT/LC08/C02/T1_L2' // Landsat 8 imagery
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  },
  
  async analyzeSoilHealth(latitude, longitude) {
    const endpoint = `${this.baseUrl}/soil-analysis`;
    const params = new URLSearchParams({
      lat: latitude,
      lng: longitude
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  },
  
  async getCropHealthIndex(latitude, longitude, cropType) {
    const endpoint = `${this.baseUrl}/crop-health`;
    const params = new URLSearchParams({
      lat: latitude,
      lng: longitude,
      crop: cropType
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  }
};
```

### Soil Health Card API
```javascript
// Government Soil Health Data
const soilHealthAPI = {
  baseUrl: 'https://soilhealth.dac.gov.in/api/v1',
  
  async getSoilHealthCard(farmerId) {
    const endpoint = `${this.baseUrl}/soil-health-card`;
    const params = new URLSearchParams({
      farmer_id: farmerId
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  },
  
  async getSoilRecommendations(soilData) {
    const endpoint = `${this.baseUrl}/soil-recommendations`;
    const requestBody = {
      soil_data: soilData
    };
    
    const response = await fetch(endpoint, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(requestBody)
    });
    
    return response.json();
  }
};
```

---

## 5. Firebase APIs (Backend Services)

### Firebase Authentication
```javascript
// User Authentication and Management
const firebaseAuth = {
  async signUpWithPhone(phoneNumber) {
    const appVerifier = new firebase.auth.RecaptchaVerifier('recaptcha-container');
    
    const confirmationResult = await firebase.auth().signInWithPhoneNumber(
      phoneNumber, 
      appVerifier
    );
    
    return confirmationResult;
  },
  
  async verifyOTP(confirmationResult, otp) {
    const result = await confirmationResult.confirm(otp);
    return result.user;
  },
  
  async updateFarmerProfile(userId, profileData) {
    const userRef = firebase.firestore().collection('users').doc(userId);
    await userRef.update({
      profile: profileData,
      updatedAt: firebase.firestore.FieldValue.serverTimestamp()
    });
  }
};
```

### Firebase Firestore
```javascript
// Real-time Database Operations
const firestoreDB = {
  async saveDiseaseAnalysis(analysisData) {
    const analysisRef = firebase.firestore().collection('diseaseAnalysis');
    const docRef = await analysisRef.add({
      ...analysisData,
      timestamp: firebase.firestore.FieldValue.serverTimestamp()
    });
    return docRef.id;
  },
  
  async getMarketData(crop, location) {
    const marketRef = firebase.firestore().collection('marketData');
    const snapshot = await marketRef
      .where('crop', '==', crop)
      .where('location', '==', location)
      .orderBy('timestamp', 'desc')
      .limit(1)
      .get();
    
    return snapshot.docs[0]?.data();
  },
  
  async saveLandAnalysis(landData) {
    const landRef = firebase.firestore().collection('landAnalysis');
    const docRef = await landRef.add({
      ...landData,
      timestamp: firebase.firestore.FieldValue.serverTimestamp()
    });
    return docRef.id;
  }
};
```

### Firebase Cloud Functions
```javascript
// Serverless Backend Functions
const cloudFunctions = {
  async processImageAnalysis(imageUrl, cropType, location) {
    const functionUrl = 'https://us-central1-project-kisan.cloudfunctions.net/analyzeCropImage';
    
    const response = await fetch(functionUrl, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        imageUrl: imageUrl,
        cropType: cropType,
        location: location
      })
    });
    
    return response.json();
  },
  
  async generateMarketInsights(crop, location) {
    const functionUrl = 'https://us-central1-project-kisan.cloudfunctions.net/generateMarketInsights';
    
    const response = await fetch(functionUrl, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        crop: crop,
        location: location
      })
    });
    
    return response.json();
  }
};
```

---

## 6. Third-Party Agricultural APIs

### Crop Calendar API
```javascript
// Agricultural Calendar and Timing
const cropCalendarAPI = {
  baseUrl: 'https://api.cropcalendar.com/v1',
  
  async getCropCalendar(crop, location) {
    const endpoint = `${this.baseUrl}/calendar`;
    const params = new URLSearchParams({
      crop: crop,
      location: location
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  },
  
  async getOptimalPlantingTime(crop, location) {
    const endpoint = `${this.baseUrl}/planting-time`;
    const params = new URLSearchParams({
      crop: crop,
      location: location
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  }
};
```

### Pest Management API
```javascript
// Pest and Disease Information
const pestManagementAPI = {
  baseUrl: 'https://api.pestmanagement.org/v1',
  
  async getPestInfo(pestName, crop) {
    const endpoint = `${this.baseUrl}/pest-info`;
    const params = new URLSearchParams({
      pest: pestName,
      crop: crop
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  },
  
  async getTreatmentOptions(disease, crop, location) {
    const endpoint = `${this.baseUrl}/treatments`;
    const params = new URLSearchParams({
      disease: disease,
      crop: crop,
      location: location
    });
    
    const response = await fetch(`${endpoint}?${params}`);
    return response.json();
  }
};
```

---

## 7. API Integration Strategy

### Rate Limiting and Caching
```javascript
// API Rate Limiting and Caching Strategy
const apiManager = {
  cache: new Map(),
  rateLimits: new Map(),
  
  async makeAPICall(apiFunction, cacheKey, ttl = 300000) { // 5 minutes TTL
    // Check cache first
    if (this.cache.has(cacheKey)) {
      const cached = this.cache.get(cacheKey);
      if (Date.now() - cached.timestamp < ttl) {
        return cached.data;
      }
    }
    
    // Check rate limits
    if (this.isRateLimited(apiFunction.name)) {
      throw new Error('Rate limit exceeded');
    }
    
    // Make API call
    const data = await apiFunction();
    
    // Cache the result
    this.cache.set(cacheKey, {
      data: data,
      timestamp: Date.now()
    });
    
    return data;
  },
  
  isRateLimited(apiName) {
    const now = Date.now();
    const limits = this.rateLimits.get(apiName) || [];
    
    // Remove old entries
    const recentCalls = limits.filter(timestamp => now - timestamp < 60000);
    this.rateLimits.set(apiName, recentCalls);
    
    return recentCalls.length >= 100; // 100 calls per minute limit
  }
};
```

### Error Handling and Fallbacks
```javascript
// Robust Error Handling for Rural Connectivity
const errorHandler = {
  async withFallback(primaryAPI, fallbackAPI, params) {
    try {
      return await primaryAPI(params);
    } catch (error) {
      console.warn('Primary API failed, trying fallback:', error);
      
      try {
        return await fallbackAPI(params);
      } catch (fallbackError) {
        console.error('Both APIs failed:', fallbackError);
        throw new Error('Service temporarily unavailable');
      }
    }
  },
  
  async withRetry(apiFunction, maxRetries = 3) {
    for (let i = 0; i < maxRetries; i++) {
      try {
        return await apiFunction();
      } catch (error) {
        if (i === maxRetries - 1) throw error;
        await new Promise(resolve => setTimeout(resolve, 1000 * (i + 1)));
      }
    }
  }
};
```

---

## 8. API Security and Authentication

### API Key Management
```javascript
// Secure API Key Management
const apiKeyManager = {
  keys: {
    GOOGLE_AI: process.env.GOOGLE_AI_API_KEY,
    WEATHER: process.env.WEATHER_API_KEY,
    EARTH_ENGINE: process.env.EARTH_ENGINE_API_KEY,
    FIREBASE: process.env.FIREBASE_CONFIG
  },
  
  getKey(service) {
    return this.keys[service];
  },
  
  validateKey(service, key) {
    return this.keys[service] === key;
  }
};
```

### Request Signing and Validation
```javascript
// Request Security
const requestSecurity = {
  async signRequest(payload, secret) {
    const crypto = require('crypto');
    const signature = crypto
      .createHmac('sha256', secret)
      .update(JSON.stringify(payload))
      .digest('hex');
    
    return signature;
  },
  
  async validateRequest(payload, signature, secret) {
    const expectedSignature = await this.signRequest(payload, secret);
    return signature === expectedSignature;
  }
};
```

---

## 9. API Monitoring and Analytics

### Performance Monitoring
```javascript
// API Performance Tracking
const apiMonitor = {
  metrics: {
    responseTimes: new Map(),
    errorRates: new Map(),
    successRates: new Map()
  },
  
  async trackAPICall(apiName, startTime) {
    const duration = Date.now() - startTime;
    
    if (!this.metrics.responseTimes.has(apiName)) {
      this.metrics.responseTimes.set(apiName, []);
    }
    
    this.metrics.responseTimes.get(apiName).push(duration);
    
    // Keep only last 100 measurements
    if (this.metrics.responseTimes.get(apiName).length > 100) {
      this.metrics.responseTimes.get(apiName).shift();
    }
  },
  
  getAverageResponseTime(apiName) {
    const times = this.metrics.responseTimes.get(apiName) || [];
    return times.length > 0 ? times.reduce((a, b) => a + b, 0) / times.length : 0;
  }
};
```

This comprehensive API integration guide ensures Project Kisan can leverage all available data sources while maintaining reliability, security, and performance for rural users with limited connectivity. 