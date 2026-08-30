---
layout: page
title: " "
share-title: "Fotios Ntinas — Data Science Portfolio"
share-description: "Mathematician turned data scientist. Learning-to-rank on Azure ML, short-term rental market analysis in Power BI, and credit risk classification — each written up in full, methods and failures included."
---

<div class="home-container">
  <div class="home-sidebar">
    <img src="{{ site.baseurl }}/assets/images/profile_pic.png" alt="Fotios Ntinas" class="profile-photo">
    <div class="sidebar-name">Fotios Ntinas</div>
    <div class="sidebar-location">📍 Serres, Greece</div>
    <div class="sidebar-links">
      <a href="https://www.linkedin.com/in/fotios-ntinas" target="_blank">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" viewBox="0 0 16 16"><path d="M0 1.146C0 .513.526 0 1.175 0h13.65C15.474 0 16 .513 16 1.146v13.708c0 .633-.526 1.146-1.175 1.146H1.175C.526 16 0 15.487 0 14.854V1.146zm4.943 12.248V6.169H2.542v7.225h2.401zm-1.2-8.212c.837 0 1.358-.554 1.358-1.248-.015-.709-.52-1.248-1.342-1.248-.822 0-1.359.54-1.359 1.248 0 .694.521 1.248 1.327 1.248h.016zm4.908 8.212V9.359c0-.216.016-.432.08-.586.173-.431.568-.878 1.232-.878.869 0 1.216.662 1.216 1.634v3.865h2.401V9.25c0-2.22-1.184-3.252-2.764-3.252-1.274 0-1.845.7-2.165 1.193v.025h-.016a5.54 5.54 0 0 1 .016-.025V6.169h-2.4c.03.678 0 7.225 0 7.225h2.4z"/></svg>
        LinkedIn
      </a>
      <a href="https://github.com/ntinasf" target="_blank">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" viewBox="0 0 16 16"><path d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.012 8.012 0 0 0 16 8c0-4.42-3.58-8-8-8z"/></svg>
        GitHub
      </a>
      <a href="mailto:ntinasfotios@gmail.com">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" viewBox="0 0 16 16"><path d="M0 4a2 2 0 0 1 2-2h12a2 2 0 0 1 2 2v8a2 2 0 0 1-2 2H2a2 2 0 0 1-2-2V4Zm2-1a1 1 0 0 0-1 1v.217l7 4.2 7-4.2V4a1 1 0 0 0-1-1H2Zm13 2.383-4.708 2.825L15 11.105V5.383Zm-.034 6.876-5.64-3.471L8 9.583l-1.326-.795-5.64 3.47A1 1 0 0 0 2 13h12a1 1 0 0 0 .966-.741ZM1 11.105l4.708-2.897L1 5.383v5.722Z"/></svg>
        Email
      </a>
    </div>
  </div>
  
  <div class="home-content">
    <h1 class="content-title">Fotios Ntinas</h1>
    <p>
      Welcome! I'm Fotis, a mathematician by training who transitioned into data science. I enjoy working with numbers and solving real problems. Data science is how I get to do both.
    </p>
    
    <h2 class="section-title">About Me</h2>
    <p>
      My foundation is in mathematics, with a Bachelor's from Aristotle University of Thessaloniki and a Master's in Data Science & Machine Learning from Hellenic Open University. But my real training came from nearly a decade as a mathematics tutor, where I learned that technical depth only matters if you can make it clear and usable for others.
    </p>
    <p>
      I apply that analytical foundation using statistical rigor alongside tools like Python, SQL and Power BI to extract insights that are clear, defensible and actually answer the questions that matter. I focus on tackling real-world problems and turning data into actionable insights.
    </p>
    
    <h2 class="section-title">Featured Projects</h2>
    <h3>Ranking Short-Term Rental Listings</h3>
    <p>
      A learning-to-rank system over 44,684 Inside Airbnb listings for Athens, Thessaloniki and Crete. It builds a demand-proxy target out of forward calendar availability, audits it for leakage, and reaches 0.753 NDCG@10 on a sealed test fold against a 0.552 random floor — then measures where it fails and specifies the A/B test that would settle the rest.
    </p>
    <p><strong>Tools:</strong> Python, LightGBM, MLflow, Azure ML, Docker</p>
    <p>
      <a href="{{ site.baseurl }}/projects/rental-ranking" class="btn btn-primary">View Case Study</a>
      <a href="https://github.com/ntinasf/rental-ranking" class="btn btn-info" target="_blank">View Repository</a>
    </p>


    <h3>Thessaloniki Airbnb Market Analysis</h3>
    <p>
      An evidence-based analysis of 4,124 short-term rental listings, examining host ecosystem dynamics, geographic performance patterns and market sustainability to inform tourism policy recommendations.
    </p>
    <p><strong>Tools:</strong> Python, Power BI, Statistical Hypothesis Testing, Geospatial Analysis, Feature Engineering</p>
    <p>
      <a href="{{ site.baseurl }}/projects/thessaloniki-airbnb" class="btn btn-primary">View Case Study</a>
    </p>


    <h3>Credit Risk Classification</h3>
    <p>
      An ensemble ML model with an interactive demo for predicting loan applicant credit risk.
    </p>
    <p><strong>Tools:</strong> Python, Scikit-learn, MLflow, Streamlit</p>
    <p>
      <a href="{{ site.baseurl }}/projects/credit-risk-classifier" class="btn btn-primary">View Case Study</a>
      <a href="https://credit-risk-h6wzqyepauzgpp29kypx9e.streamlit.app" class="btn btn-info" target="_blank">Try Demo</a>
    </p>


    <h2 class="section-title">Certifications</h2>
    <ul class="cert-list">
      <li>
        <a href="https://learn.microsoft.com/en-us/users/fotiosntinas-2744/credentials/b4cc5a568434835" target="_blank">Microsoft Certified: Azure Data Scientist Associate</a>
        <span class="cert-meta">DP-100 &middot; issued August 2025, valid to August 2027</span>
      </li>
      <li>
        <a href="https://learn.microsoft.com/en-us/users/fotiosntinas-2744/credentials/eb403d15f7b662de" target="_blank">Microsoft Certified: Power BI Data Analyst Associate</a>
        <span class="cert-meta">PL-300 &middot; issued March 2025, valid to March 2027</span>
      </li>
    </ul>
  </div>
</div>
