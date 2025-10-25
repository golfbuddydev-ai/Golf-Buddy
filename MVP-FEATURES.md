# Golf Buddy MVP - User Stories & Features

## Week 1 Planning Document
**Created**: October 2025
**Target**: 6-week MVP launch

---

## User Demographics & Skill Levels

### Skill Breakdown
- **Beginners**: Handicap 25+ (New to golf, learning fundamentals)
- **Advanced Beginners**: Handicap 17-25 (Comfortable playing, improving consistency)
- **Intermediate Players**: Handicap 10-17 (Experienced, competitive potential)
- **Advanced Players**: Handicap <10 (Skilled, tournament-ready)

### Target Users
**Primary Users** (70% of user base):
- Beginners (HDCP 25+)
- Advanced Beginners (HDCP 17-25)
- Casual golfers playing 2-4x per month
- Seeking social, friendly rounds
- Struggle to find playing partners at their skill level

**Secondary Users** (30% of user base):
- Intermediate Players (HDCP 10-17)
- Advanced Players (HDCP <10)
- Play weekly or more
- Want skill-matched, competitive rounds
- Seeking consistent playing partners

---

## Round Experience Preferences

Users can define their preferred round style:
- **Competitive**: Focused play, tracking scores, improving handicap
- **Friendly**: Relaxed but serious, some score tracking
- **Social**: Conversation-focused, casual pace
- **Casual**: Fun-first, minimal score pressure
- **Cart**: Riding preferred
- **Walking**: Walking the course preferred

---

## Golf Personality Tags (NEW - MVP Feature)

**Why**: Skill match ≠ personality match. Help golfers find compatible playing partners beyond handicap.

**Implementation**: During profile setup, users select 3-5 personality traits (optional but encouraged):

### Personality Tag Options:
- 🗣️ **Chatty** / 🤫 **Quiet** - Communication style on course
- 🍺 **19th hole enthusiast** / 🏃 **Quick exit** - Post-round socializing
- 😄 **Jokes around** / 😐 **Focused play** - Round atmosphere preference
- 📸 **Takes photos** / 👁️ **Stays present** - Social media/documentation style
- 🎵 **Music on cart** / 🔇 **Silence preferred** - Audio environment
- ⚡ **Fast pace** / 🐢 **Leisurely pace** - Playing speed
- 🏆 **Keeps strict score** / 🤷 **Casual counting** - Score tracking intensity
- 🍻 **Drinks while playing** / 💧 **Stays sober** - Alcohol preference

**Matching Logic**: 
- Tags are weighted in match algorithm alongside skill level and location
- Users can filter by compatible personality traits
- "High compatibility" badge shown when 4+ tags align

---

## Core User Stories (MVP)

### Authentication
- **US-001**: As a new user, I can sign up with email/password so I can create an account
- **US-002**: As a returning user, I can log in with my credentials so I can access my profile
- **US-003**: As a user, I can sign in with Google for faster access

### Profile Management
- **US-004**: As a user, I can create my profile with name, photo, handicap, and preferred courses
- **US-005**: As a user, I can define my preferred round experience (competitive, social, friendly, casual, cart/walking)
- **US-006**: As a user, I can select personality tags that describe my golf style (optional)
- **US-007**: As a user, I can edit my profile details and personality tags at any time
- **US-008**: As a user, I can view other users' profiles including their personality tags

### Matching System
- **US-009**: As a user, I can see nearby golfers within 50km based on my location
- **US-010**: As a user, I can filter matches by skill level (Beginner 25+, Adv Beginner 17-25, Intermediate 10-17, Advanced <10)
- **US-011**: As a user, I can search for golfers by preferred round experience (competitive, social, friendly, casual)
- **US-012**: As a user, I can filter matches by personality tags (chatty, quiet, 19th hole, etc.)
- **US-013**: As a user, I can see "compatibility score" based on aligned personality tags and preferences
- **US-014**: As a user, I can search for golfers by cart/walking preference
- **US-015**: As a user, I can view match details (distance, handicap, round style, personality tags, preferred courses)
- **US-016**: As a user, I must match with someone before I can message them (prevents spam)

### Chat/Messaging
- **US-017**: As a user, I can only send messages to golfers I've matched with (mutual connection required)
- **US-018**: As a user, I can see my chat history with each matched contact
- **US-019**: As a user, I receive notifications for new messages from matched golfers

### Events/Rounds
- **US-020**: As a user, I can create a golf round event (course, date, time, player count, round style)
- **US-021**: As a user, I can invite multiple matched golfers to my events
- **US-022**: As a user, I can only join events if I'm matched with the creator and receive an invite (invite-only, no public events)
- **US-023**: As a user, I receive notifications for event invites from matched golfers
- **US-024**: As a user, I can accept or decline event invitations

---

## MVP Feature Checklist

### ✅ MUST HAVE (Week 1-6)
- [ ] Email/Password authentication
- [ ] Google Sign-In
- [ ] User profile (name, photo, handicap, preferred courses)
- [ ] User profile - round experience preferences (competitive, friendly, social, casual, cart/walking)
- [ ] User profile - personality tags selection (3-5 tags, optional)
- [ ] Location-based matching (Google Maps API)
- [ ] Skill-based filtering (4 tiers: Beginner 25+, Adv Beginner 17-25, Intermediate 10-17, Advanced <10)
- [ ] Round experience filtering (competitive, social, friendly, casual, cart/walking)
- [ ] Personality tag filtering (chatty, quiet, 19th hole, focused, etc.)
- [ ] Compatibility scoring based on tag alignment
- [ ] Match-gating for messaging (can only chat with matched users)
- [ ] Real-time chat (Firebase Realtime Database)
- [ ] Event creation with round style definition
- [ ] Event invitation system (can invite multiple matched golfers)
- [ ] Invite-only events (cannot join unless matched and invited)
- [ ] Push notifications (FCM) for messages and event invites
- [ ] Bottom navigation (Profile, Matches, Chat, Events)

### 🎯 NICE TO HAVE (Post-Launch v1.1)
- [ ] Full Golf Personality Quiz: Buzzfeed-style 10-question quiz with shareable results
- [ ] Personality badges: "Social Swinger", "Competitive Crusher", "Chill Chipper", etc.
- [ ] Quiz retake: Update personality type anytime
- [ ] Social sharing: Share quiz results to Instagram/Twitter for viral growth
- [ ] Profile verification badges
- [ ] Course reviews/ratings
- [ ] Weather integration for event dates
- [ ] Round history tracking
- [ ] Favorite matched golfers (bookmark system)
- [ ] Block/report users
- [ ] In-app photo sharing in chat
- [ ] Event reminders (24hr before tee time)

### 🚀 FUTURE (v2.0+ - Monetization & Growth)
- [ ] E-commerce Integration: Golf equipment shop, course pro shop partnerships
- [ ] Premium Subscription: Advanced matching filters, unlimited events, verified badge, personality insights
- [ ] Tee time booking integration: Direct booking through app
- [ ] Post-round features: Scorecards, stats tracking, handicap calculation
- [ ] Tournament creation: Organize group competitions
- [ ] Affiliate partnerships: Golf brands, courses, instructors
- [ ] Sponsored events: Brand-sponsored rounds with prizes
- [ ] Marketplace: Sell/buy used clubs, apparel
- [ ] Course partnerships: Discounted rates for Golf Buddy users
- [ ] Instructor network: Connect with local golf pros
- [ ] AI-powered matching: Machine learning to improve compatibility predictions over time

---

## Monetization Strategy (Future Discussion)

### Phase 1 (MVP): Free
- Build user base, prove concept
- Focus on engagement and retention
- Test personality matching hypothesis

### Phase 2 (Post-Launch): Freemium
- **Free tier**: Basic matching, 5 events/month, standard search, basic personality tags
- **Premium ($10/year)**: Unlimited events, advanced filters, full personality quiz, compatibility insights, verified badge, priority support

### Phase 3 (Scale): E-commerce & Partnerships
- **Affiliate commissions**: Golf equipment sales (5-10% commission)
- **Course partnerships**: Booking fees, discounted tee times
- **Sponsored content**: Featured courses, equipment reviews
- **Premium marketplace**: Transaction fees on used equipment sales
- **Data insights**: Sell anonymized personality/preference data to golf brands for product development

---

## Technical Requirements

### Android (Kotlin)
- Min SDK: 24 (Android 7.0)
- Target SDK: 34 (Android 14)
- Architecture: MVVM
- Navigation: Jetpack Navigation Component
- UI: Jetpack Compose (modern, reactive UI for personality tag selection)

### iOS (Swift)
- Min iOS: 15.0
- UI: SwiftUI
- Architecture: MVVM
- Navigation: NavigationStack

### Backend (Firebase)
- Authentication: Email/Password, Google
- Database: Firestore (profiles, matches, events, invitations, personality tags)
- Storage: Firebase Storage (profile photos)
- Messaging: Firebase Cloud Messaging (push notifications)
- Analytics: Firebase Analytics (track tag selection rates, compatibility conversion)

---

## Key Differentiators

✅ **Personality-based matching**: First golf app to match by vibe, not just skill  
✅ **Match-gated messaging**: Prevents spam, creates safe environment  
✅ **Invite-only events**: Quality over quantity, trusted connections  
✅ **Round experience matching**: Beyond skill level, match playing styles  
✅ **4-tier skill system**: Clear, inclusive categories for all golfers  
✅ **Cart/walking preference**: Practical filter for round compatibility  
✅ **Compatibility scoring**: Data-driven matching with transparent algorithm  

---

## Success Metrics (Post-Launch)

- **User Acquisition**: 100 signups in first month
- **Profile Completion**: 70% of users complete personality tags (measure adoption)
- **Engagement**: 50% weekly active users
- **Matching**: Average 5 matches viewed per session
- **Match Rate**: 35% of profile views result in mutual matches (up from 30% without tags)
- **Messaging**: 45% of matches result in chat conversations (up from 40%)
- **Tag Effectiveness**: Track which tag combinations lead to highest event creation rates
- **Events**: 15% of users create/join events monthly
- **Retention**: 65% 30-day retention rate (up from 60% due to better matches)

---

## Out of Scope (MVP)

❌ Full 10-question personality quiz (v1.1 feature)  
❌ Personality badges/labels (v1.1 feature)  
❌ AI-powered matching recommendations (v2.0+)  
❌ Tee time booking  
❌ Payment processing (subscription)  
❌ Social feed/posts  
❌ Video chat  
❌ Advanced analytics dashboard  
❌ Multi-language support  
❌ Desktop/web version  
❌ E-commerce marketplace (Phase 3)  
❌ Tournament system (Phase 3)
