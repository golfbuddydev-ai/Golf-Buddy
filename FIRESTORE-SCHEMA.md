# Golf Buddy - Firestore Database Schema

## Database Structure Overview
**Database Type**: Cloud Firestore (NoSQL)  
**Location**: asia-southeast1 (Singapore)  
**Security**: Start in test mode (will secure in Week 2)

---

## Collections & Document Structure

### 1. **users** (Collection)
Stores user profile information

**Document ID**: Firebase Auth UID (auto-generated)

**Fields**:
```json
{
  "userId": "string (Firebase Auth UID)",
  "email": "string",
  "displayName": "string",
  "photoURL": "string (Firebase Storage URL)",
  "handicap": "number (0-54)",
  "skillLevel": "string (beginner | advancedBeginner | intermediate | advanced)",
  "preferredCourses": ["array of strings"],
  "roundExperience": {
    "style": "string (competitive | friendly | social | casual)",
    "pace": "string (cart | walking)"
  },
  "personalityTags": [
    "array of strings",
    "Example: ['chatty', '19thHole', 'jokesAround', 'takesPhotos', 'musicOnCart', 'fastPace', 'strictScore', 'drinksWhilePlaying']"
  ],
  "location": {
    "latitude": "number",
    "longitude": "number",
    "city": "string",
    "country": "string"
  },
  "createdAt": "timestamp",
  "updatedAt": "timestamp",
  "isActive": "boolean",
  "lastSeen": "timestamp"
}
```

**Indexes Needed**:
- `skillLevel` (for filtering)
- `location.latitude` + `location.longitude` (for geo queries)
- Composite: `skillLevel` + `isActive`

---

### 2. **matches** (Collection)
Stores mutual connections between users

**Document ID**: Auto-generated or composite `userId1_userId2` (alphabetically sorted)

**Fields**:
```json
{
  "matchId": "string (auto-generated)",
  "user1Id": "string (userId)",
  "user2Id": "string (userId)",
  "user1Name": "string",
  "user2Name": "string",
  "user1PhotoURL": "string",
  "user2PhotoURL": "string",
  "compatibilityScore": "number (0-100)",
  "sharedTags": ["array of matching personality tags"],
  "matchedAt": "timestamp",
  "lastMessageAt": "timestamp (for sorting chat list)",
  "unreadCount": {
    "user1": "number",
    "user2": "number"
  },
  "status": "string (active | blocked | deleted)"
}
```

**Indexes Needed**:
- `user1Id` (to query user's matches)
- `user2Id` (to query user's matches)
- Composite: `user1Id` + `matchedAt` (for chronological list)
- Composite: `user2Id` + `matchedAt`

---

### 3. **messages** (Subcollection under matches)
Stores chat messages between matched users

**Path**: `matches/{matchId}/messages/{messageId}`

**Document ID**: Auto-generated

**Fields**:
```json
{
  "messageId": "string (auto-generated)",
  "senderId": "string (userId)",
  "senderName": "string",
  "receiverId": "string (userId)",
  "text": "string",
  "timestamp": "timestamp",
  "isRead": "boolean",
  "readAt": "timestamp (nullable)"
}
```

**Indexes Needed**:
- `timestamp` (for chronological ordering)

**Why Subcollection?**: Keeps messages organized per match, efficient queries, automatic cleanup when match deleted

---

### 4. **events** (Collection)
Stores golf round events created by users

**Document ID**: Auto-generated

**Fields**:
```json
{
  "eventId": "string (auto-generated)",
  "creatorId": "string (userId)",
  "creatorName": "string",
  "creatorPhotoURL": "string",
  "courseName": "string",
  "courseAddress": "string (optional)",
  "date": "timestamp",
  "time": "string (e.g., '9:00 AM')",
  "playerCount": "number (2-4)",
  "currentPlayers": "number",
  "roundStyle": "string (competitive | friendly | social | casual)",
  "invitedUserIds": ["array of userIds"],
  "acceptedUserIds": ["array of userIds"],
  "declinedUserIds": ["array of userIds"],
  "status": "string (upcoming | completed | cancelled)",
  "createdAt": "timestamp",
  "updatedAt": "timestamp"
}
```

**Indexes Needed**:
- `creatorId` (to query user's created events)
- `date` (for sorting by upcoming dates)
- Composite: `status` + `date` (for active upcoming events)
- Array: `invitedUserIds` (to query events user is invited to)
- Array: `acceptedUserIds` (to query events user accepted)

---

### 5. **notifications** (Collection)
Stores push notification logs and in-app notifications

**Document ID**: Auto-generated

**Fields**:
```json
{
  "notificationId": "string (auto-generated)",
  "userId": "string (recipient userId)",
  "type": "string (newMatch | newMessage | eventInvite | eventAccepted | eventReminder)",
  "title": "string",
  "body": "string",
  "data": {
    "matchId": "string (optional)",
    "eventId": "string (optional)",
    "senderId": "string (optional)"
  },
  "isRead": "boolean",
  "createdAt": "timestamp",
  "readAt": "timestamp (nullable)"
}
```

**Indexes Needed**:
- `userId` + `createdAt` (for user's notification feed)
- Composite: `userId` + `isRead` + `createdAt`

---

### 6. **userPreferences** (Collection)
Stores app settings and preferences per user

**Document ID**: Same as userId (one-to-one mapping)

**Fields**:
```json
{
  "userId": "string",
  "notificationSettings": {
    "newMatches": "boolean",
    "messages": "boolean",
    "eventInvites": "boolean",
    "eventReminders": "boolean"
  },
  "privacySettings": {
    "showLocation": "boolean",
    "showHandicap": "boolean"
  },
  "searchPreferences": {
    "maxDistance": "number (km, default: 50)",
    "handicapRange": "number (default: 5)"
  },
  "updatedAt": "timestamp"
}
```

---

## Data Flow Examples

### **Scenario 1: User Signup**
1. Create document in `users` collection (via Firebase Auth + Firestore)
2. Create document in `userPreferences` with default settings
3. User completes profile → Update `users` document

### **Scenario 2: Matching Flow**
1. User A views User B's profile
2. User A "matches" with User B → Check if User B already matched User A
3. If mutual: Create document in `matches` collection
4. Calculate `compatibilityScore` based on shared `personalityTags`
5. Create notification in `notifications` for both users
6. Send push notification via FCM

### **Scenario 3: Sending Message**
1. Check if `matches/{matchId}` exists (users are matched)
2. If yes: Create document in `matches/{matchId}/messages` subcollection
3. Update `matches/{matchId}.lastMessageAt`
4. Increment `matches/{matchId}.unreadCount` for receiver
5. Create notification in `notifications` for receiver
6. Send push notification via FCM

### **Scenario 4: Creating Event**
1. Create document in `events` collection
2. Add matched user IDs to `invitedUserIds` array
3. Create notifications for each invited user
4. Send push notifications via FCM

### **Scenario 5: Accepting Event Invite**
1. Update `events/{eventId}.acceptedUserIds` (add userId)
2. Increment `events/{eventId}.currentPlayers`
3. Create notification for event creator
4. Send push notification to creator

---

## Security Considerations (Week 2)

**Current**: Test mode (open read/write until [30 days from creation])

**Production Rules** (to implement):
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Users can read their own data and other users' public profiles
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth.uid == userId;
    }
    
    // Matches: Only matched users can read/write
    match /matches/{matchId} {
      allow read: if request.auth.uid in resource.data.keys();
      allow create: if request.auth != null;
      allow update: if request.auth.uid in resource.data.keys();
    }
    
    // Messages: Only matched users can read/write messages
    match /matches/{matchId}/messages/{messageId} {
      allow read: if request.auth.uid in get(/databases/$(database)/documents/matches/$(matchId)).data.keys();
      allow create: if request.auth.uid in get(/databases/$(database)/documents/matches/$(matchId)).data.keys();
    }
    
    // Events: Creator can write, invited users can read
    match /events/{eventId} {
      allow read: if request.auth.uid in resource.data.invitedUserIds || request.auth.uid == resource.data.creatorId;
      allow create: if request.auth != null;
      allow update: if request.auth.uid == resource.data.creatorId || request.auth.uid in resource.data.invitedUserIds;
    }
    
    // Notifications: Users can only read their own
    match /notifications/{notificationId} {
      allow read: if request.auth.uid == resource.data.userId;
      allow write: if false; // Only backend can write
    }
    
    // Preferences: Users can only read/write their own
    match /userPreferences/{userId} {
      allow read, write: if request.auth.uid == userId;
    }
  }
}
```

---

## Database Size Estimates (6 months)

**Assumptions**: 500 users, 50% weekly active

- **users**: 500 docs × ~2KB = 1MB
- **matches**: ~2,500 matches × ~1KB = 2.5MB
- **messages**: ~50,000 messages × ~500B = 25MB
- **events**: ~500 events × ~1.5KB = 750KB
- **notifications**: ~10,000 notifications × ~500B = 5MB
- **userPreferences**: 500 docs × ~500B = 250KB

**Total**: ~35MB (well within free tier: 1GB storage)

**Reads/Writes**: Estimated 100K reads/day, 20K writes/day (within free tier limits)

---

## Next Steps (Week 2)

- [ ] Implement security rules
- [ ] Set up Firestore indexes in Firebase Console
- [ ] Create Cloud Functions for:
  - Calculate compatibility scores
  - Send push notifications
  - Clean up old messages/notifications
- [ ] Add data validation rules
- [ ] Set up Firebase Analytics events

---

**Schema Version**: 1.0  
**Last Updated**: October 2025
