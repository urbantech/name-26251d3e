# Product Requirements Document (PRD)

## Web-Based Audio Streaming API for DJs & Podcasters

---

```json
{
  "document_metadata": {
    "title": "Web-Based Audio Streaming API for DJs & Podcasters",
    "version": "1.0",
    "date": "2025-06-03",
    "author": "Product Architecture Team",
    "status": "Final",
    "document_type": "Product Requirements Document"
  },

  "executive_summary": {
    "overview": "A comprehensive RESTful API service enabling DJs and podcasters to stream live audio via Shoutcast relay, manage on-demand playlists with direct file uploads, host podcasts with custom domains, and monetize content through subscriptions and ad insertion.",
    "problem_statement": "DJs and podcasters lack a unified, scalable platform that combines live streaming relay, on-demand content management, podcast hosting with custom branding, and integrated monetization—all accessible via a developer-friendly API.",
    "solution": "Build a cloud-native, horizontally scalable API service that ingests Shoutcast live feeds, transcodes audio to HLS/MP3, manages playlists and podcast episodes (with direct uploads or external URLs), provisions custom domains with automated TLS, and provides subscription-based monetization with dynamic ad insertion.",
    "key_benefits": [
      "Unified platform for live streaming, on-demand playlists, and podcast hosting",
      "Direct file upload with automated HLS transcoding for seamless playback",
      "Custom domain support with automated DNS and TLS certificate provisioning",
      "Flexible monetization via Stripe subscriptions and dynamic ad insertion",
      "Comprehensive analytics for streams, playlists, and podcast episodes",
      "Horizontally scalable architecture supporting tens of thousands of concurrent listeners",
      "GDPR-compliant data handling with 'Right to Be Forgotten' support"
    ],
    "success_criteria": [
      "99.9% uptime SLA for API and streaming endpoints",
      "Support for 10,000+ concurrent listeners per live stream",
      "Sub-2-second latency for HLS segment generation",
      "100% automated custom domain provisioning within 10 minutes",
      "Zero data breaches and full GDPR compliance",
      "90%+ DJ satisfaction score on ease of use and reliability"
    ]
  },

  "product_overview": {
    "vision": "To become the leading API-first platform for DJs and podcasters, empowering creators with professional-grade live streaming, on-demand content delivery, and monetization tools—all accessible through a simple, secure, and scalable API.",
    "mission": "Provide a robust, developer-friendly infrastructure that abstracts the complexity of live audio relay, HLS transcoding, podcast RSS generation, and payment processing, enabling creators to focus on content and audience engagement.",
    "core_capabilities": [
      {
        "capability": "Live Streaming Relay (Shoutcast Integration)",
        "description": "Ingest live audio feeds from Shoutcast sources, segment into HLS chunks via FFmpeg, and multicast to all connected listeners with real-time metadata (current track, bitrate, listener count)."
      },
      {
        "capability": "On-Demand Playlist Management",
        "description": "Allow DJs to upload MP3 files directly or reference external URLs, automatically transcode to HLS, and serve via HLS manifest or continuous MP3 stream endpoints."
      },
      {
        "capability": "Podcast Hosting & RSS with Custom Domains",
        "description": "Enable DJs to create podcast series, upload episodes, and serve valid RSS 2.0 feeds on custom domains (e.g., podcast.djname.com) with automated DNS and TLS provisioning."
      },
      {
        "capability": "Monetization & Ad Insertion",
        "description": "Provide subscription plans via Stripe, enforce paywalls on streams/playlists/episodes, and insert ads (pre-roll, mid-roll) dynamically during live and on-demand playback."
      },
      {
        "capability": "Metadata & Analytics",
        "description": "Expose real-time and historical analytics for live streams (listener counts), playlists (play counts), and podcasts (download counts) with date-range queries and exportable reports."
      },
      {
        "capability": "Scalable, Secure, & Reliable Infrastructure",
        "description": "Horizontally scalable Kubernetes-based architecture, JWT authentication, role-based access control, encryption at rest and in transit, GDPR compliance, and 99.9% uptime SLA."
      }
    ],
    "target_market": {
      "primary_segments": [
        "Professional DJs and radio hosts streaming live sets",
        "Podcasters seeking custom-branded RSS feeds and monetization",
        "Music producers and labels distributing on-demand content",
        "Developer teams building DJ dashboards, mobile apps, or embedded players"
      ],
      "market_size": "Global live streaming market projected at $247B by 2027; podcast market at $94B by 2028.",
      "competitive_advantage": [
        "API-first design for seamless integration into existing platforms",
        "Direct file upload with automated HLS transcoding (no external encoding required)",
        "Automated custom domain provisioning (DNS + TLS) in under 10 minutes",
        "Integrated monetization (Stripe subscriptions + ad insertion) out-of-the-box",
        "Real-time analytics and metadata for live streams, playlists, and podcasts"
      ]
    }
  },

  "target_users_and_personas": {
    "primary_personas": [
      {
        "persona_name": "DJ / Stream Operator",
        "demographics": {
          "age_range": "25-45",
          "occupation": "Professional DJ, Radio Host, Music Producer",
          "technical_skill": "Intermediate to Advanced (comfortable with APIs, Shoutcast, and audio software)"
        },
        "goals": [
          "Stream live DJ sets to thousands of listeners with minimal latency",
          "Upload and manage on-demand playlists for fans to replay",
          "Host a podcast series with a custom domain (e.g., podcast.djname.com)",
          "Monetize content via subscriptions and ad revenue",
          "Access detailed analytics on listener engagement and revenue"
        ],
        "pain_points": [
          "Existing platforms lack integrated live streaming + on-demand + podcast hosting",
          "Manual HLS transcoding is time-consuming and error-prone",
          "Custom domain setup for podcasts requires technical DNS/TLS knowledge",
          "Limited monetization options (no built-in subscriptions or ad insertion)",
          "Poor analytics visibility (no real-time listener counts or play metrics)"
        ],
        "user_journey": [
          "Register account and obtain API credentials (JWT)",
          "Register Shoutcast source via POST /v1/streams (provide URL, mount, stream_key)",
          "API spins up Relay Worker Pod to ingest feed and generate HLS segments",
          "Upload MP3 files via POST /v1/uploads/audio for on-demand playlists",
          "Create playlist via POST /v1/playlists (reference uploaded files or external URLs)",
          "Create podcast series via POST /v1/podcasts (specify custom domain)",
          "Add episodes via POST /v1/podcasts/{podcast_id}/episodes",
          "Configure monetization via POST /v1/monetization/plans and subscriptions",
          "Assign ad markers via POST /v1/streams/{stream_id}/ads",
          "View analytics via GET /v1/analytics/streams/{stream_id}, /playlists/{playlist_id}, /podcasts/{podcast_id}"
        ]
      },
      {
        "persona_name": "API Consumer / Developer",
        "demographics": {
          "age_range": "22-40",
          "occupation": "Full-Stack Developer, Mobile App Developer, DevOps Engineer",
          "technical_skill": "Advanced (proficient in REST APIs, React, FastAPI, PostgreSQL)"
        },
        "goals": [
          "Integrate live streaming, playlists, and podcast feeds into a custom DJ dashboard or mobile app",
          "Automate DNS and TLS provisioning for custom podcast domains",
          "Implement payment flows using Stripe subscriptions",
          "Build real-time listener dashboards with WebSocket or polling",
          "Ensure secure, scalable, and GDPR-compliant data handling"
        ],
        "pain_points": [
          "Lack of comprehensive API documentation and SDKs",
          "Complex authentication and authorization flows",
          "Difficulty integrating live HLS streams with CDN caching",
          "Manual DNS/TLS setup for custom domains is error-prone",
          "Limited support for real-time analytics and webhooks"
        ],
        "user_journey": [
          "Review OpenAPI spec and Swagger UI documentation",
          "Obtain API credentials (JWT) via POST /v1/auth/login",
          "Integrate live stream endpoints (GET /v1/streams/{stream_id}/live.m3u8) into video.js or HLS.js player",
          "Implement file upload UI (multipart/form-data) for POST /v1/uploads/audio",
          "Build playlist management UI (CRUD operations on /v1/playlists)",
          "Automate custom domain provisioning via POST /v1/domains and poll GET /v1/domains/{domain_id} for status",
          "Integrate Stripe Checkout for subscription plans via POST /v1/monetization/subscriptions",
          "Display real-time analytics via GET /v1/analytics/* endpoints",
          "Handle Stripe webhooks at /v1/monetization/webhook for subscription updates"
        ]
      },
      {
        "persona_name": "Listener (Implicit)",
        "demographics": {
          "age_range": "18-55",
          "occupation": "Music Fan, Podcast Subscriber, Event Attendee",
          "technical_skill": "Basic (uses web browsers, mobile apps, podcast apps)"
        },
        "goals": [
          "Listen to live DJ sets with minimal buffering",
          "Replay on-demand playlists at any time",
          "Subscribe to podcasts via custom RSS feeds in Apple Podcasts, Spotify, etc.",
          "Access premium content via paid subscriptions",
          "Enjoy ad-supported free content"
        ],
        "pain_points": [
          "Buffering and latency during live streams",
          "Broken or outdated podcast RSS feeds",
          "Confusing payment flows for premium content",
          "Intrusive or poorly timed ads"
        ],
        "user_journey": [
          "Discover DJ's live stream link (e.g., https://api.example.com/v1/streams/{stream_id}/live.m3u8)",
          "Open link in HLS-compatible player (web, mobile, smart speaker)",
          "Authenticate via JWT (if private stream or paywalled)",
          "Listen to live stream with real-time metadata (current track, bitrate)",
          "Subscribe to podcast via custom RSS URL (e.g., https://podcast.djname.com/rss.xml)",
          "Download episodes via podcast app (Apple Podcasts, Spotify, Overcast)",
          "Upgrade to premium subscription via Stripe Checkout for ad-free access"
        ]
      }
    ],
    "secondary_personas": [
      {
        "persona_name": "Admin / Platform Operator",
        "role": "Manage all DJs, monitor system health, handle billing and compliance",
        "goals": [
          "View global analytics across all streams, playlists, and podcasts",
          "Manage user accounts and permissions (RBAC)",
          "Monitor Relay Worker health and resource utilization",
          "Handle GDPR data deletion requests",
          "Ensure 99.9% uptime and respond to incidents"
        ]
      }
    ]
  },

  "functional_requirements": {
    "feature_categories": [
      {
        "category": "Authentication & Authorization",
        "priority": "Critical",
        "user_stories": [
          {
            "id": "AUTH-001",
            "title": "User Registration",
            "as_a": "DJ",
            "i_want_to": "register a new account with email and password",
            "so_that": "I can access the API and manage my streams, playlists, and podcasts",
            "acceptance_criteria": [
              "POST /v1/auth/register accepts email, password, and name",
              "Password must be ≥8 characters with uppercase, lowercase, number, and special character",
              "Email must be unique; return 409 Conflict if duplicate",
              "Return 201 Created with user_id, email, and role (default: 'dj')",
              "Send email verification link (optional)"
            ],
            "api_endpoint": "POST /v1/auth/register"
          },
          {
            "id": "AUTH-002",
            "title": "User Login",
            "as_a": "DJ",
            "i_want_to": "log in with my email and password",
            "so_that": "I receive a JWT access token and refresh token",
            "acceptance_criteria": [
              "POST /v1/auth/login accepts email and password",
              "Return 200 OK with access_token (JWT, RS256, 24h expiry), refresh_token, and expires_in",
              "Return 401 Unauthorized if credentials invalid",
              "Log failed login attempts for security monitoring"
            ],
            "api_endpoint": "POST /v1/auth/login"
          },
          {
            "id": "AUTH-003",
            "title": "Token Refresh",
            "as_a": "DJ",
            "i_want_to": "refresh my JWT access token using a refresh token",
            "so_that": "I can maintain authenticated sessions without re-entering credentials",
            "acceptance_criteria": [
              "POST /v1/auth/refresh accepts refresh_token",
              "Return 200 OK with new access_token and expires_in",
              "Return 401 Unauthorized if refresh_token invalid or expired",
              "Rotate refresh tokens on each refresh (optional)"
            ],
            "api_endpoint": "POST /v1/auth/refresh"
          },
          {
            "id": "AUTH-004",
            "title": "Password Reset",
            "as_a": "DJ",
            "i_want_to": "request a password reset link via email",
            "so_that": "I can reset my password if forgotten",
            "acceptance_criteria": [
              "PUT /v1/auth/password-reset with email sends reset link",
              "PUT /v1/auth/password-reset with reset_token and new_password updates password",
              "Return 200 OK on success",
              "Return 400 Bad Request if reset_token invalid or expired"
            ],
            "api_endpoint": "PUT /v1/auth/password-reset"
          },
          {
            "id": "AUTH-005",
            "title": "Role-Based Access Control (RBAC)",
            "as_a": "Admin",
            "i_want_to": "enforce role-based permissions on API endpoints",
            "so_that": "DJs can only access their own resources and Admins have global access",
            "acceptance_criteria": [
              "JWT includes 'role' claim ('dj' or 'admin')",
              "Middleware validates JWT and checks scopes (e.g., streams:read, playlists:write)",
              "Return 403 Forbidden if user lacks required scope or tries to access another user's resource",
              "Admins bypass ownership checks for global visibility"
            ],
            "implementation_notes": "Use middleware to extract JWT, verify signature, and check scopes before allowing access to protected endpoints."
          }
        ]
      },
      {
        "category": "Live Streaming Relay (Shoutcast Integration)",
        "priority": "Critical",
        "user_stories": [
          {
            "id": "STREAM-001",
            "title": "Register Shoutcast Source",
            "as_a": "DJ",
            "i_want_to": "register a Shoutcast source URL and credentials",
            "so_that": "the API spins up a Relay Worker to ingest and multicast my live feed",
            "acceptance_criteria": [
              "POST /v1/streams accepts name, shoutcast_url, mount, stream_key, genre, description,