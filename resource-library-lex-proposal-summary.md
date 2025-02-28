# AT Protocol Community Resource Library
## Summary

Written by Claude, guided by Dame.

### TL;DR
We propose creating a comprehensive, community-maintained directory of AT Protocol and Bluesky resources using a set of Lexicons for `atproto.community`. The system allows users to submit resources, vote and review them, propose amendments to keep information current, and detect duplicates—all while respecting the AT Protocol's repository ownership model.

## The Problem

Currently, information about AT Protocol tools and resources is scattered across various websites that:
- Often contain advertisements
- Vary greatly in completeness and accuracy
- Lack community maintenance capabilities
- Don't utilize the AT Protocol's native capabilities

## The Solution

We propose a community-curated resource directory built on AT Protocol using custom Lexicons that enable:

1. **Resource Submission** - Anyone can submit tools, clients, feed generators, etc.
2. **Community Curation** - Voting, reviews, and collections
3. **Collaborative Maintenance** - Amendment process for updating information
4. **Duplicate Prevention** - Intelligent detection and alias linking

## Core Components

### 1. Primary Resource Record

The foundation is the `community.atproto.resource` record containing:
- Resource name, URL, and description
- Submitter and creator DIDs
- Category and status using token references
- Tags, version info, and repository links

```json
{
  "$type": "community.atproto.resource",
  "name": "Example Feed Generator",
  "uri": "https://example.com/feed",
  "submitter": "did:plc:user123",
  "creator": "did:plc:creator456",
  "description": "A feed generator for Bluesky",
  "category": "community.atproto.resource.category.feedGenerator",
  "status": "community.atproto.resource.status.active",
  "tags": ["feeds", "algorithmic"],
  "createdAt": "2025-03-01T10:15:30Z"
}
```

### 2. Community Curation Mechanisms

**Voting**
- Simple up/down votes on resources
- Aggregation of vote totals

**Reviews**
- Detailed text reviews with ratings (1-5 stars)
- Attribution to reviewer

**Collections**
- Curated lists of resources with notes
- Thematic organization (e.g., "Best Photography Feed Generators")

### 3. Amendment System

AT Protocol repositories can only be modified by their owners, so we've designed a system to enable community updates:

1. **Amendment Proposal** - A user proposes changes to a resource
2. **Community Voting** - Other users vote on the amendment
3. **Resolution** - Automatic or admin resolution based on votes
4. **Admin View Merging** - Creation of a merged canonical view

### 4. Duplicate Handling

To prevent redundant entries:

1. **URI Normalization** - Detect similar URLs (removing www, trailing slashes, etc.)
2. **Resource Aliases** - Link alternative URLs to a canonical resource
3. **Merge Requests** - Process for combining duplicate entries

## How It Works: A Real Example

The lifecycle of "Skyline," a hypothetical Bluesky client:

1. **Initial submission** by Alice with basic info
2. **Voting and reviews** from the community
3. **Amendment proposal** when the app updates and changes URL
4. **Community voting** approves the amendment
5. **Admin view creation** merges in the changes
6. **Duplicate submission detection** when another user tries to add the same app
7. **Status update** to "discontinued" when the app is eventually retired
8. **Historical collection** preserves the resource in a curated list

## Implementation Overview

The system requires:

1. **Backend Indexing**
   - Resource and amendment tracking
   - Vote tallying and resolution
   - URL normalization for duplicate detection

2. **User Experience**
   - Clear amendment and voting interfaces
   - Transparent display of merged information
   - Duplicate detection during submission

## Benefits

1. **Centralized Discovery** - One reliable place to find AT Protocol resources
2. **Community Maintained** - Accuracy through collective knowledge
3. **Structured Data** - Consistent format for all resource information
4. **Attribution** - Proper credit to creators and contributors
5. **Historical Record** - Documentation of the ecosystem's evolution

## Next Steps

1. Develop backend indexing service
2. Create frontend components
3. Establish moderation policies
4. Seed with initial resources
5. Engage with tool creators for verification

For full details, please refer to the [complete proposal document](link-to-full-proposal).
