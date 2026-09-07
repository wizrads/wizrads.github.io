---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
sitemap: false
---

{% include base_path %}

Education
======
* Ph.D. in Medical Physics, University of Wisconsin–Madison, in progress (started Sep 2024)
  * Advisor: Bryan Bednarz, PhD
  * M.S. in Medical Physics (Clinical/Research), Aug 2026
  * Research focus: radiotherapy optimization, AI-assisted treatment planning, and machine learning for medical physics
* B.S. in Physics, minor in Mathematics, Boston College, 2021

* American Board of Radiology (ABR) Part 1, passed 2026

Professional experience
======
* Sep 2024 – present: Graduate Research Assistant
  * University of Wisconsin–Madison, Department of Medical Physics, Madison, WI
  * Integrated catheter position and dwell time optimization for focal dose escalation in prostate HDR brachytherapy, including an in-house Python brachytherapy TPS with inverse optimization and directional IMBT capability
  * Robust simultaneous optimization of catheter positions and dwell times in a single inverse-planning framework
  * Implicit neural representations for end-to-end differentiable brachytherapy dose calculation, trained in PyTorch
  * Deep learning autosegmentation for preclinical radiopharmaceutical therapy dosimetry (Swin UNETR, DSC 0.91), deployed as an internal tool
  * LLM-based analysis of linac downtime; 3D SAM-Body foundation segmentation for markerless CyberKnife patient positioning
  * Preclinical electron FLASH collaboration with Stanford, including an open-source FLASH TPS and the conversion of a clinical TrueBeam to FLASH mode

* Dec 2021 – Jul 2024: Medical Physicist Assistant
  * Stanford Health Care / Stanford Cancer Center / Stanford University, San Jose and Palo Alto, CA
  * Supported clinical physics workflows across SRS, SBRT, IMRT, VMAT, SGRT, IGRT, treatment planning QA, imaging QA, and patient-specific QA
  * Developed Python and C# (ESAPI) scripts to improve treatment planning, QA, and radiation oncology workflow efficiency
  * Implemented patient-specific 3D-printing workflows for electron cutouts, photon blocks, bolus devices, and clinical radiotherapy accessories
  * Supported clinical translation of AVATAR, VR patient education, and 3D-printed shielding projects from prototype to clinical use

* Jun 2021 – Nov 2021: Medical Physicist Assistant
  * Northern Light Health, Eastern Maine Medical Center Cancer Center, Bangor, ME
  * Independently performed daily linac warmup and QA for Varian TrueBeam, Clinac iX, and Novalis Tx with BrainLab ExacTrac
  * Performed ArcCHECK VMAT QA for patient-specific plan verification
  * Developed Python workflows in RayStation and created patient-specific Cerrobend electron blocks

Skills
======
* Programming and AI/ML
  * Python, PyTorch, C# (ESAPI), MATLAB
  * Deep learning, nnU-Net, Swin UNETR, SAM-Body, implicit neural representations
  * AI-assisted coding workflows, LLM evaluation
* Clinical and research physics
  * HDR brachytherapy (inverse and robust optimization)
  * Radiopharmaceutical therapy (RPT) dosimetry
  * FLASH / UHDR, TOPAS Monte Carlo
  * Deformable image registration, image analysis
* Dosimetry and QA
  * Instrumentation, detectors, OSLDs, film dosimetry
  * Treatment planning systems (clinical and custom open-source)
  * Patient-specific, daily, monthly, and annual QA
* Prototyping and immersive technology
  * 3D printing, 3D scanning, anthropomorphic phantom fabrication, virtual reality

Grants
======
* 2024–2025: Stanford CATALYST ($500,000) — software and workflow contributor, AVATAR
* 2024–2025: SQIMM Award ($6,000), Department of Radiation Oncology, Stanford University — Laser Gantry
* 2024–2025: SQIMM Award ($6,000), Department of Radiation Oncology, Stanford University — AVATAR for RefleXion
* 2023–2024: SQIMM Award ($15,000), Department of Radiation Oncology, Stanford University — scripting for streamlined 3D-printed patient specific devices
* 2022–2023: SQIMM Award ($10,000), Department of Radiation Oncology, Stanford University — interactive virtual tour for patient navigation and education

Awards, fellowships, and honors
======
* 2026: MC2 Best Abstract Award, KAMPiNA/KSMP/JSMP Joint Symposium — LLM based linac downtime analysis
* 2026: Best in Physics (Therapy), AAPM Annual Meeting — LLM based linac downtime analysis
* 2026: Best in Physics (Therapy), AAPM Annual Meeting — combination Lu-177 PSMA RPT and Ir-192 HDR prostate brachytherapy
* 2026: Best in Physics (Radiopharmaceuticals), AAPM Annual Meeting — pelvic-vertebral 3D-printed anthropomorphic phantom
* 2026: Theranostics and Particle Therapy (ITPT) Conference Travel Award ($500)
* 2025: AAPM/RSNA Doctoral Graduate Fellowship, AAPM
* 2025: Standard Imaging Travel Award, Standard Imaging
* 2025: Vilas Research Travel Award, UW–Madison
* 2022: Arthur Boyer Award for Innovation in Medical Physics Education, AAPM Annual Meeting
* 2020–2021: Dean's List First Honors, Boston College
* 2020–2021: Undergraduate Research Fellowship, Boston College
* 2018: Undergraduate Research Fellowship, Boston College
* 2017: Bernard A. Dinatale Memorial Scholarship, Physics Excellence

Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
Talks
======
  <ul>{% for post in site.talks reversed %}
    {% include archive-single-talk-cv.html  %}
  {% endfor %}</ul>
  
Teaching
======
  <ul>{% for post in site.teaching reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>

Professional affiliations
======
* 2024–present: Member, North Central Chapter of the AAPM
* 2024–present: Member, American Society for Radiation Oncology (ASTRO)
* 2022–2024: Member, Northern California Chapter of the AAPM
* 2022–present: Member, American Association of Physicists in Medicine (AAPM)

Service and leadership
======
* 2026–present: Reviewer, *Medical Physics*
* 2026–present: Reviewer, *Journal of Applied Clinical Medical Physics*
* 2025–present: Guest member, AAPM Mentorship Subcommittee (MENTORSC)
* 2025–present: Guest member, AAPM TG-450 Information Technology Education in Radiation Oncology
* 2025–present: AAPM Graduate Student Liaison, Society of Directors of Academic Medical Physics Programs (SDAMPP)
* 2025–present: Voting member, AAPM Student and Trainee Subcommittee (STSC)
* 2025–present: Question developer, Radiological Exam Development Group
* 2022–2025: Voting member, AAPM Medical Physicist Assistants Subcommittee (MPASC)
* 2022–present: Mentee, AAPM Mentorship Program
* 2022–2024: Volunteer, Northern California Chapter of the AAPM
