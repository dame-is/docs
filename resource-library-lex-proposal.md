# AT Protocol Community Resource Library
## Comprehensive Lexicon Proposal for atproto.community

## Table of Contents

1. [Introduction](#introduction)
2. [Core Lexicon Design](#core-lexicon-design)
   - [Resource Record](#resource-record)
   - [Token System](#token-system)
   - [Categories and Status Tokens](#categories-and-status-tokens)
3. [Community Curation System](#community-curation-system)
   - [Resource Voting](#resource-voting)
   - [Resource Reviews](#resource-reviews)
   - [Resource Collections](#resource-collections)
4. [Community-Driven Amendment Process](#community-driven-amendment-process)
   - [Amendment Proposal](#amendment-proposal)
   - [Community Voting](#community-voting)
   - [Amendment Resolution](#amendment-resolution)
   - [Admin View Merging](#admin-view-merging)
5. [Duplicate Resource Handling](#duplicate-resource-handling)
   - [URI Normalization](#uri-normalization)
   - [Resource Aliases](#resource-aliases)
   - [Resource Merge Requests](#resource-merge-requests)
6. [Complete Lexicon Reference](#complete-lexicon-reference)
   - [Primary Resource Lexicons](#primary-resource-lexicons)
   - [Curation Lexicons](#curation-lexicons)
   - [Amendment Lexicons](#amendment-lexicons)
   - [Duplicate Handling Lexicons](#duplicate-handling-lexicons)
7. [Real-World Example: The Life of a Community Resource](#real-world-example-the-life-of-a-community-resource)
8. [Implementation Considerations](#implementation-considerations)
   - [Indexing Requirements](#indexing-requirements)
   - [User Experience Recommendations](#user-experience-recommendations)
   - [Privacy and Moderation](#privacy-and-moderation)
9. [Conclusion and Next Steps](#conclusion-and-next-steps)

## Introduction

The AT Protocol and Bluesky ecosystem is evolving rapidly, with numerous tools, clients, feed generators, and other resources being created by the community. Currently, information about these resources is scattered across various websites, often loaded with advertisements and varying in completeness and accuracy.

This proposal defines a comprehensive lexicon system for `atproto.community` to create a community-maintained directory of AT Protocol and Bluesky resources. By leveraging the decentralized nature of the AT Protocol while implementing community curation mechanisms, this system will provide:

1. A centralized discovery point for AT Protocol resources
2. Community-driven content maintenance and quality control
3. A structured data format that other applications can build upon
4. Attribution to original creators
5. Up-to-date status information about resources

The proposed lexicon system respects the AT Protocol's repository ownership model while enabling collaborative maintenance through a carefully designed amendment and voting system.

## Core Lexicon Design

### Resource Record

The core of the system is the `community.atproto.resource` record type, which contains metadata about an AT Protocol or Bluesky-related resource.

```json
{
  "lexicon": 1,
  "id": "community.atproto.resource",
  "description": "A record containing metadata about an AT Protocol or Bluesky-related resource or tool.",
  "defs": {
    "main": {
      "type": "record",
      "description": "A shared resource for the AT Protocol community.",
      "key": "tid",
      "record": {
        "type": "object",
        "required": ["name", "uri", "submitter", "createdAt"],
        "properties": {
          "name": {
            "type": "string",
            "description": "Name of the resource",
            "maxLength": 120
          },
          "uri": {
            "type": "string",
            "format": "uri",
            "description": "URI of the resource"
          },
          "submitter": {
            "type": "string",
            "format": "did",
            "description": "DID of the account that submitted this resource"
          },
          "creator": {
            "type": "string",
            "format": "did",
            "description": "DID of the account that created this resource (if known)"
          },
          "description": {
            "type": "string",
            "description": "Description of the resource",
            "maxLength": 3000
          },
          "images": {
            "type": "array",
            "description": "Images showcasing the resource",
            "items": {
              "type": "ref",
              "ref": "#image"
            }
          },
          "tags": {
            "type": "array",
            "description": "Tags for categorizing the resource",
            "items": {
              "type": "string",
              "maxLength": 50
            },
            "maxLength": 10
          },
          "createdAt": {
            "type": "string",
            "format": "datetime",
            "description": "Timestamp when this resource record was created"
          },
          "lastUpdated": {
            "type": "string",
            "format": "datetime",
            "description": "Timestamp when this resource record was last updated"
          },
          "status": {
            "type": "string",
            "description": "Current status of the resource",
            "knownValues": [
              "community.atproto.resource.status.active",
              "community.atproto.resource.status.deprecated",
              "community.atproto.resource.status.discontinued"
            ]
          },
          "category": {
            "type": "string",
            "description": "Primary category of the resource",
            "knownValues": [
              "community.atproto.resource.category.feedGenerator",
              "community.atproto.resource.category.client",
              "community.atproto.resource.category.bot",
              "community.atproto.resource.category.pds",
              "community.atproto.resource.category.developer",
              "community.atproto.resource.category.documentation",
              "community.atproto.resource.category.other"
            ]
          },
          "license": {
            "type": "string",
            "description": "License information for the resource (if applicable)",
            "maxLength": 200
          },
          "version": {
            "type": "string",
            "description": "Version information for the resource (if applicable)",
            "maxLength": 50
          },
          "repositoryUri": {
            "type": "string",
            "format": "uri",
            "description": "URI to the source code repository (if applicable)"
          }
        }
      }
    },
    "image": {
      "type": "object",
      "required": ["image", "alt"],
      "properties": {
        "image": {
          "type": "blob",
          "accept": ["image/*"],
          "maxSize": 1000000
        },
        "alt": {
          "type": "string",
          "description": "Alt text for the image",
          "maxLength": 300
        }
      }
    }
  }
}
```

### Token System

The token system uses the Lexicon token type to define semantic values for categories and statuses. This approach provides:

1. **Extensibility**: New categories or statuses can be added without breaking existing code or data
2. **Semantic Clarity**: Each token has a clear definition and documentation 
3. **Namespaced Organization**: The hierarchical structure prevents naming collisions

### Categories and Status Tokens

**Status Tokens:**

```json
{
  "lexicon": 1,
  "id": "community.atproto.resource.status.active",
  "defs": {
    "main": {
      "type": "token",
      "description": "The resource is currently active and available."
    }
  }
}

{
  "lexicon": 1,
  "id": "community.atproto.resource.status.deprecated",
  "defs": {
    "main": {
      "type": "token",
      "description": "The resource still works but is no longer maintained or recommended."
    }
  }
}

{
  "lexicon": 1,
  "id": "community.atproto.resource.status.discontinued",
  "defs": {
    "main": {
      "type": "token",
      "description": "The resource is no longer available or functional."
    }
  }
}
```

**Category Tokens:**

```json
{
  "lexicon": 1,
  "id": "community.atproto.resource.category.feedGenerator",
  "defs": {
    "main": {
      "type": "token",
      "description": "A custom feed generator for Bluesky."
    }
  }
}

{
  "lexicon": 1,
  "id": "community.atproto.resource.category.client",
  "defs": {
    "main": {
      "type": "token",
      "description": "A client application for accessing Bluesky or AT Protocol services."
    }
  }
}

{
  "lexicon": 1,
  "id": "community.atproto.resource.category.bot",
  "defs": {
    "main": {
      "type": "token",
      "description": "An automated bot account on the AT Protocol network."
    }
  }
}

{
  "lexicon": 1,
  "id": "community.atproto.resource.category.pds",
  "defs": {
    "main": {
      "type": "token",
      "description": "A Personal Data Server implementation or hosting service."
    }
  }
}

{
  "lexicon": 1,
  "id": "community.atproto.resource.category.developer",
  "defs": {
    "main": {
      "type": "token",
      "description": "Development tools, libraries, or utilities for AT Protocol."
    }
  }
}

{
  "lexicon": 1,
  "id": "community.atproto.resource.category.documentation",
  "defs": {
    "main": {
      "type": "token",
      "description": "Documentation, guides, or educational materials for AT Protocol."
    }
  }
}

{
  "lexicon": 1,
  "id": "community.atproto.resource.category.other",
  "defs": {
    "main": {
      "type": "token",
      "description": "Resources that don't fit into other categories."
    }
  }
}
```

## Community Curation System

To enable community participation in resource curation, three additional lexicons are provided:

### Resource Voting

```json
{
  "lexicon": 1,
  "id": "community.atproto.resourceVote",
  "description": "A record indicating a user's vote on a resource.",
  "defs": {
    "main": {
      "type": "record",
      "description": "A vote on a resource.",
      "key": "tid",
      "record": {
        "type": "object",
        "required": ["subject", "direction", "createdAt"],
        "properties": {
          "subject": {
            "type": "string",
            "format": "at-uri",
            "description": "URI of the resource being voted on"
          },
          "direction": {
            "type": "string",
            "description": "Direction of the vote",
            "enum": ["up", "down"]
          },
          "createdAt": {
            "type": "string",
            "format": "datetime",
            "description": "Timestamp when the vote was created"
          }
        }
      }
    }
  }
}
```

### Resource Reviews

```json
{
  "lexicon": 1,
  "id": "community.atproto.resourceReview",
  "description": "A record containing a user's review of a resource.",
  "defs": {
    "main": {
      "type": "record",
      "description": "A user's review of a resource.",
      "key": "tid",
      "record": {
        "type": "object",
        "required": ["subject", "text", "rating", "createdAt"],
        "properties": {
          "subject": {
            "type": "string",
            "format": "at-uri",
            "description": "URI of the resource being reviewed"
          },
          "text": {
            "type": "string",
            "description": "Text content of the review",
            "maxLength": 5000
          },
          "rating": {
            "type": "integer",
            "description": "Rating from 1-5 stars",
            "minimum": 1,
            "maximum": 5
          },
          "createdAt": {
            "type": "string",
            "format": "datetime",
            "description": "Timestamp when the review was created"
          }
        }
      }
    }
  }
}
```

### Resource Collections

```json
{
  "lexicon": 1,
  "id": "community.atproto.resourceCollection",
  "description": "A record containing a curated collection of resources.",
  "defs": {
    "main": {
      "type": "record",
      "description": "A curated collection of resources.",
      "key": "tid",
      "record": {
        "type": "object",
        "required": ["name", "description", "items", "creator", "createdAt"],
        "properties": {
          "name": {
            "type": "string",
            "description": "Name of the collection",
            "maxLength": 100
          },
          "description": {
            "type": "string",
            "description": "Description of the collection",
            "maxLength": 3000
          },
          "items": {
            "type": "array",
            "description": "Resources in this collection",
            "items": {
              "type": "ref",
              "ref": "#collectionItem"
            },
            "maxLength": 100
          },
          "creator": {
            "type": "string",
            "format": "did",
            "description": "DID of the account that created this collection"
          },
          "createdAt": {
            "type": "string",
            "format": "datetime",
            "description": "Timestamp when this collection was created"
          },
          "lastUpdated": {
            "type": "string",
            "format": "datetime",
            "description": "Timestamp when this collection was last updated"
          },
          "tags": {
            "type": "array",
            "description": "Tags for categorizing the collection",
            "items": {
              "type": "string",
              "maxLength": 50
            },
            "maxLength": 10
          }
        }
      }
    },
    "collectionItem": {
      "type": "object",
      "required": ["uri", "addedAt"],
      "properties": {
        "uri": {
          "type": "string",
          "format": "at-uri",
          "description": "URI of the resource"
        },
        "addedAt": {
          "type": "string",
          "format": "datetime",
          "description": "Timestamp when this resource was added to the collection"
        },
        "note": {
          "type": "string",
          "description": "Curator's note about this resource",
          "maxLength": 500
        }
      }
    }
  }
}
```

## Community-Driven Amendment Process

A key challenge for maintaining accurate resource information is that in the AT Protocol, records are stored in users' personal repositories and can only be modified by their creators. To address this constraint while enabling community maintenance, a comprehensive amendment system is proposed:

### Amendment Proposal

```json
{
  "lexicon": 1,
  "id": "community.atproto.resourceAmendment",
  "description": "A suggested amendment to an existing resource record.",
  "defs": {
    "main": {
      "type": "record",
      "description": "A proposed change to a resource record that can be voted on by the community.",
      "key": "tid",
      "record": {
        "type": "object",
        "required": ["subject", "changes", "reason", "submitter", "createdAt"],
        "properties": {
          "subject": {
            "type": "string",
            "format": "at-uri",
            "description": "URI of the resource being amended"
          },
          "changes": {
            "type": "object",
            "description": "Proposed changes to the resource",
            "properties": {
              "name": {
                "type": "string",
                "maxLength": 120
              },
              "uri": {
                "type": "string",
                "format": "uri"
              },
              "creator": {
                "type": "string",
                "format": "did"
              },
              "description": {
                "type": "string",
                "maxLength": 3000
              },
              "status": {
                "type": "string",
                "knownValues": [
                  "community.atproto.resource.status.active",
                  "community.atproto.resource.status.deprecated",
                  "community.atproto.resource.status.discontinued"
                ]
              },
              "category": {
                "type": "string",
                "knownValues": [
                  "community.atproto.resource.category.feedGenerator",
                  "community.atproto.resource.category.client",
                  "community.atproto.resource.category.bot",
                  "community.atproto.resource.category.pds",
                  "community.atproto.resource.category.developer",
                  "community.atproto.resource.category.documentation",
                  "community.atproto.resource.category.other"
                ]
              },
              "tags": {
                "type": "array",
                "items": {
                  "type": "string",
                  "maxLength": 50
                },
                "maxLength": 10
              },
              "license": {
                "type": "string",
                "maxLength": 200
              },
              "version": {
                "type": "string",
                "maxLength": 50
              },
              "repositoryUri": {
                "type": "string",
                "format": "uri"
              }
            }
          },
          "reason": {
            "type": "string",
            "description": "Reason for the proposed changes",
            "maxLength": 1000
          },
          "submitter": {
            "type": "string",
            "format": "did",
            "description": "DID of the account proposing the amendment"
          },
          "createdAt": {
            "type": "string",
            "format": "datetime",
            "description": "Timestamp when this amendment was created"
          }
        }
      }
    }
  }
}
```

### Community Voting

```json
{
  "lexicon": 1,
  "id": "community.atproto.amendmentVote",
  "description": "A vote on a proposed resource amendment.",
  "defs": {
    "main": {
      "type": "record",
      "description": "A user's vote on a proposed amendment.",
      "key": "tid",
      "record": {
        "type": "object",
        "required": ["subject", "vote", "voter", "createdAt"],
        "properties": {
          "subject": {
            "type": "string",
            "format": "at-uri",
            "description": "URI of the amendment being voted on"
          },
          "vote": {
            "type": "string",
            "description": "The vote decision",
            "enum": ["approve", "reject"]
          },
          "comment": {
            "type": "string",
            "description": "Optional comment explaining the vote",
            "maxLength": 500
          },
          "voter": {
            "type": "string",
            "format": "did",
            "description": "DID of the account casting the vote"
          },
          "createdAt": {
            "type": "string",
            "format": "datetime",
            "description": "Timestamp when this vote was cast"
          }
        }
      }
    }
  }
}
```

### Amendment Resolution

```json
{
  "lexicon": 1,
  "id": "community.atproto.amendmentResolution",
  "description": "The resolution of a proposed amendment, either by admin decision or community vote.",
  "defs": {
    "main": {
      "type": "record",
      "description": "The decision on a resource amendment proposal.",
      "key": "tid",
      "record": {
        "type": "object",
        "required": ["subject", "decision", "resolver", "createdAt"],
        "properties": {
          "subject": {
            "type": "string",
            "format": "at-uri",
            "description": "URI of the amendment being resolved"
          },
          "decision": {
            "type": "string",
            "description": "The final decision on the amendment",
            "enum": ["approved", "rejected", "partial"]
          },
          "partialChanges": {
            "type": "object",
            "description": "If decision is 'partial', specifies which changes were accepted"
          },
          "reason": {
            "type": "string",
            "description": "Explanation for the decision",
            "maxLength": 1000
          },
          "communityVotes": {
            "type": "object",
            "description": "Summary of community voting",
            "properties": {
              "approvals": {
                "type": "integer",
                "description": "Number of approval votes"
              },
              "rejections": {
                "type": "integer",
                "description": "Number of rejection votes"
              }
            }
          },
          "resolver": {
            "type": "string",
            "format": "did",
            "description": "DID of the admin account making the final decision"
          },
          "createdAt": {
            "type": "string",
            "format": "datetime",
            "description": "Timestamp when this resolution was created"
          }
        }
      }
    }
  }
}
```

### Admin View Merging

```json
{
  "lexicon": 1,
  "id": "community.atproto.resourceAdminView",
  "description": "The canonical admin view of a resource, potentially merging multiple amendments.",
  "defs": {
    "main": {
      "type": "record",
      "description": "The official merged view of a resource with all approved amendments.",
      "key": "tid",
      "record": {
        "type": "object",
        "required": ["originalUri", "mergedAt", "admin", "mergedResource"],
        "properties": {
          "originalUri": {
            "type": "string",
            "format": "at-uri",
            "description": "URI of the original resource record"
          },
          "appliedAmendments": {
            "type": "array",
            "description": "URIs of amendments that have been applied",
            "items": {
              "type": "string",
              "format": "at-uri"
            }
          },
          "mergedResource": {
            "type": "object",
            "description": "The merged resource data with all approved amendments applied",
            "required": ["name", "uri", "submitter", "createdAt"],
            "properties": {
              "name": { "type": "string" },
              "uri": { "type": "string", "format": "uri" },
              "submitter": { "type": "string", "format": "did" },
              "creator": { "type": "string", "format": "did" },
              "description": { "type": "string" },
              "images": {
                "type": "array",
                "items": {
                  "type": "ref",
                  "ref": "community.atproto.resource#image"
                }
              },
              "tags": {
                "type": "array",
                "items": { "type": "string" }
              },
              "createdAt": { "type": "string", "format": "datetime" },
              "status": { "type": "string" },
              "category": { "type": "string" },
              "license": { "type": "string" },
              "version": { "type": "string" },
              "repositoryUri": { "type": "string", "format": "uri" }
            }
          },
          "mergedAt": {
            "type": "string",
            "format": "datetime",
            "description": "Timestamp when this merged view was last updated"
          },
          "admin": {
            "type": "string",
            "format": "did",
            "description": "DID of the admin account that created this merged view"
          }
        }
      }
    }
  }
}
```

## Duplicate Resource Handling

An important aspect of resource management is handling duplicate submissions. The following lexicons provide a framework for identifying, linking, and potentially merging duplicate resources:

### URI Normalization

URI normalization will be implemented in the backend to identify similar URLs that may point to the same resource. The system will normalize URLs by:

- Removing protocols (http://, https://)
- Removing trailing slashes
- Converting to lowercase
- Removing 'www.' prefixes

### Resource Aliases

```json
{
  "lexicon": 1,
  "id": "community.atproto.resourceAlias",
  "description": "A record that connects multiple resource URIs that represent the same tool/resource.",
  "defs": {
    "main": {
      "type": "record",
      "description": "An alias linking alternative URIs for the same resource.",
      "key": "tid",
      "record": {
        "type": "object",
        "required": ["canonicalUri", "aliasUris", "submitter", "createdAt"],
        "properties": {
          "canonicalUri": {
            "type": "string",
            "format": "at-uri",
            "description": "URI of the canonical (primary) resource record"
          },
          "aliasUris": {
            "type": "array",
            "description": "Alternative URLs that point to the same resource",
            "items": {
              "type": "string",
              "format": "uri"
            },
            "maxLength": 20
          },
          "reason": {
            "type": "string",
            "description": "Explanation for why these are the same resource",
            "maxLength": 500
          },
          "submitter": {
            "type": "string",
            "format": "did",
            "description": "DID of the account that created this alias record"
          },
          "createdAt": {
            "type": "string",
            "format": "datetime",
            "description": "Timestamp when this alias was created"
          }
        }
      }
    }
  }
}
```

### Resource Merge Requests

```json
{
  "lexicon": 1,
  "id": "community.atproto.resourceMergeRequest",
  "description": "A request to merge two seemingly duplicate resources.",
  "defs": {
    "main": {
      "type": "record",
      "description": "A request to merge two seemingly duplicate resources.",
      "key": "tid",
      "record": {
        "type": "object",
        "required": ["primaryResourceUri", "duplicateResourceUri", "reason", "submitter", "createdAt"],
        "properties": {
          "primaryResourceUri": {
            "type": "string",
            "format": "at-uri",
            "description": "URI of the resource that should be kept as the primary version"
          },
          "duplicateResourceUri": {
            "type": "string",
            "format": "at-uri",
            "description": "URI of the resource that should be marked as a duplicate"
          },
          "mergeStrategy": {
            "type": "string",
            "description": "How to handle the merge",
            "enum": ["keep-primary", "merge-fields", "keep-both-with-alias"]
          },
          "fieldMergeStrategy": {
            "type": "object",
            "description": "If mergeStrategy is 'merge-fields', specifies which fields to take from which resource"
          },
          "reason": {
            "type": "string",
            "description": "Explanation for why these should be merged",
            "maxLength": 1000
          },
          "submitter": {
            "type": "string",
            "format": "did",
            "description": "DID of the account that requested the merge"
          },
          "createdAt": {
            "type": "string",
            "format": "datetime",
            "description": "Timestamp when this merge request was created"
          }
        }
      }
    }
  }
}
```

```json
{
  "lexicon": 1,
  "id": "community.atproto.resourceMergeResolution",
  "description": "The resolution of a resource merge request.",
  "defs": {
    "main": {
      "type": "record",
      "description": "The decision on a resource merge request.",
      "key": "tid",
      "record": {
        "type": "object",
        "required": ["subject", "decision", "resolver", "createdAt"],
        "properties": {
          "subject": {
            "type": "string",
            "format": "at-uri",
            "description": "URI of the merge request being resolved"
          },
          "decision": {
            "type": "string",
            "description": "The final decision on the merge request",
            "enum": ["approved", "rejected", "modified"]
          },
          "resultingResourceUri": {
            "type": "string",
            "format": "at-uri",
            "description": "URI of the resulting primary resource after the merge"
          },
          "createdAliases": {
            "type": "array",
            "description": "URIs of any alias records created as part of this resolution",
            "items": {
              "type": "string",
              "format": "at-uri"
            }
          },
          "reason": {
            "type": "string",
            "description": "Explanation for the decision",
            "maxLength": 1000
          },
          "resolver": {
            "type": "string",
            "format": "did",
            "description": "DID of the admin that resolved the merge request"
          },
          "createdAt": {
            "type": "string",
            "format": "datetime",
            "description": "Timestamp when this resolution was created"
          }
        }
      }
    }
  }
}
```

## Complete Lexicon Reference

### Primary Resource Lexicons
- `community.atproto.resource` - Core resource record

### Category and Status Tokens
- `community.atproto.resource.status.*` - Resource status tokens
- `community.atproto.resource.category.*` - Resource category tokens

### Curation Lexicons
- `community.atproto.resourceVote` - Voting on resources
- `community.atproto.resourceReview` - Detailed reviews
- `community.atproto.resourceCollection` - Curated collections

### Amendment Lexicons
- `community.atproto.resourceAmendment` - Proposed changes
- `community.atproto.amendmentVote` - Votes on amendments
- `community.atproto.amendmentResolution` - Resolution decisions
- `community.atproto.resourceAdminView` - Merged canonical view

### Duplicate Handling Lexicons
- `community.atproto.resourceAlias` - Alternative URLs for same resource
- `community.atproto.resourceMergeRequest` - Request to merge duplicates
- `community.atproto.resourceMergeResolution` - Merge decisions

## Real-World Example: The Life of a Community Resource

To illustrate how these lexicons work together in practice, let's follow the lifecycle of a hypothetical resource: "Skyline," a popular Bluesky client application.

### 1. Initial Submission

**Alice**, a community member, discovers the Skyline client and wants to add it to the resource directory:

1. **Resource Creation**:
   Alice fills out a submission form with the following information:

   ```json
   {
     "$type": "community.atproto.resource",
     "name": "Skyline",
     "uri": "https://skyline.example/app",
     "submitter": "did:plc:alice123",
     "creator": "did:plc:dev456",
     "description": "A sleek, feature-rich client for Bluesky with advanced filtering options.",
     "category": "community.atproto.resource.category.client",
     "status": "community.atproto.resource.status.active",
     "tags": ["client", "mobile", "desktop"],
     "repositoryUri": "https://github.com/example/skyline",
     "createdAt": "2025-03-01T10:15:30.123Z"
   }
   ```

   This record is stored in Alice's personal repository with a unique TID (timestamp identifier).

2. **Initial Discovery and Voting**:
   The resource appears in the `atproto.community` directory. Other users begin to discover it:

   - **Bob** finds Skyline useful and upvotes it:
     ```json
     {
       "$type": "community.atproto.resourceVote",
       "subject": "at://did:plc:alice123/community.atproto.resource/1h9tm2hcdz92j",
       "direction": "up",
       "createdAt": "2025-03-01T11:20:45.123Z"
     }
     ```

   - **Charlie** writes a detailed review:
     ```json
     {
       "$type": "community.atproto.resourceReview",
       "subject": "at://did:plc:alice123/community.atproto.resource/1h9tm2hcdz92j",
       "text": "Skyline has become my go-to Bluesky client. The interface is intuitive and the filtering options are powerful...",
       "rating": 5,
       "createdAt": "2025-03-01T12:30:20.123Z"
     }
     ```

   The `atproto.community` index aggregates these votes and reviews to display alongside the resource.

3. **Information Update Required**:
   A month later, Skyline releases a significant update and moves to a new domain. The creator of Skyline doesn't have access to Alice's repository, so they can't update the original record. **Dana**, a community member, notices this and proposes an amendment:

   ```json
   {
     "$type": "community.atproto.resourceAmendment",
     "subject": "at://did:plc:alice123/community.atproto.resource/1h9tm2hcdz92j",
     "changes": {
       "uri": "https://skyline-app.example/download",
       "description": "A sleek, feature-rich client for Bluesky with advanced filtering options and new AI-powered content discovery.",
       "version": "2.0"
     },
     "reason": "Skyline moved to a new domain and released version 2.0 with AI features",
     "submitter": "did:plc:dana789",
     "createdAt": "2025-04-05T09:30:15.123Z"
   }
   ```

   This amendment is stored in Dana's repository, not modifying Alice's original record.

4. **Community Voting on Amendment**:
   The community begins voting on Dana's proposed amendment:

   - **Bob** approves the amendment:
     ```json
     {
       "$type": "community.atproto.amendmentVote",
       "subject": "at://did:plc:dana789/community.atproto.resourceAmendment/1k3jd7zkx9q0s",
       "vote": "approve",
       "comment": "I can confirm this is the new official URL",
       "voter": "did:plc:bob456",
       "createdAt": "2025-04-05T10:45:20.123Z"
     }
     ```

   - Several other community members also vote, resulting in 12 approvals and 0 rejections.

5. **Amendment Resolution**:
   After sufficient votes are collected, an automatic resolution is triggered:

   ```json
   {
     "$type": "community.atproto.amendmentResolution",
     "subject": "at://did:plc:dana789/community.atproto.resourceAmendment/1k3jd7zkx9q0s",
     "decision": "approved",
     "reason": "Community consensus with 12 approvals and 0 rejections",
     "communityVotes": {
       "approvals": 12,
       "rejections": 0
     },
     "resolver": "did:plc:admin001",
     "createdAt": "2025-04-06T08:15:30.123Z"
   }
   ```

6. **Admin View Creation**:
   Following the resolution, the system creates an admin view that merges the original record with the approved amendment:

   ```json
   {
     "$type": "community.atproto.resourceAdminView",
     "originalUri": "at://did:plc:alice123/community.atproto.resource/1h9tm2hcdz92j",
     "appliedAmendments": [
       "at://did:plc:dana789/community.atproto.resourceAmendment/1k3jd7zkx9q0s"
     ],
     "mergedResource": {
       "name": "Skyline",
       "uri": "https://skyline-app.example/download",
       "submitter": "did:plc:alice123",
       "creator": "did:plc:dev456",
       "description": "A sleek, feature-rich client for Bluesky with advanced filtering options and new AI-powered content discovery.",
       "category": "community.atproto.resource.category.client",
       "status": "community.atproto.resource.status.active",
       "tags": ["client", "mobile", "desktop"],
       "repositoryUri": "https://github.com/example/skyline",
       "version": "2.0",
       "createdAt": "2025-03-01T10:15:30.123Z"
     },
     "mergedAt": "2025-04-06T08:20:15.123Z",
     "admin": "did:plc:admin001"
   }
   ```

   Now when users visit the Skyline resource page on `atproto.community`, they see this merged view with the latest information, not Alice's original record.

7. **Duplicate Submission and Resolution**:
   Several months later, **Evan**, who isn't aware of the existing Skyline entry, tries to submit a new resource:

   ```json
   {
     "$type": "community.atproto.resource",
     "name": "Skyline Client",
     "uri": "https://skyline-app.example/",
     "submitter": "did:plc:evan234",
     "description": "Official Bluesky client with AI features",
     "category": "community.atproto.resource.category.client",
     "status": "community.atproto.resource.status.active",
     "tags": ["client", "ai"],
     "createdAt": "2025-06-15T14:25:10.123Z"
   }
   ```

   During submission, the system detects a potential duplicate through URI normalization. Evan is shown the existing resource and chooses to create an alias instead:

   ```json
   {
     "$type": "community.atproto.resourceAlias",
     "canonicalUri": "at://did:plc:alice123/community.atproto.resource/1h9tm2hcdz92j",
     "aliasUris": ["https://skyline-app.example/"],
     "reason": "Same app, just the homepage URL instead of download page",
     "submitter": "did:plc:evan234",
     "createdAt": "2025-06-15T14:30:45.123Z"
   }
   ```

   The alias is automatically approved and linked to the canonical resource.

8. **Status Change and Deprecation**:
   A year later, Skyline is discontinued and replaced by a new client. **Frank**, a community member, proposes an amendment to update the status:

   ```json
   {
     "$type": "community.atproto.resourceAmendment",
     "subject": "at://did:plc:alice123/community.atproto.resource/1h9tm2hcdz92j",
     "changes": {
       "status": "community.atproto.resource.status.discontinued",
       "description": "A sleek, feature-rich client for Bluesky that has been discontinued as of July 2026. Users are recommended to switch to NewSky client."
     },
     "reason": "Skyline developers announced the project's end on their blog",
     "submitter": "did:plc:frank567",
     "createdAt": "2026-07-10T11:20:30.123Z"
   }
   ```

   After community voting and approval, the admin view is updated to reflect this status change. The resource remains in the directory but is clearly marked as discontinued, helping users understand they should look for alternatives.

9. **Resource Collection Inclusion**:
   Following Skyline's discontinuation, **Grace** creates a historical collection of early Bluesky clients:

   ```json
   {
     "$type": "community.atproto.resourceCollection",
     "name": "Early Bluesky Clients (2025-2026)",
     "description": "A historical archive of the first generation of Bluesky clients that helped shape the ecosystem.",
     "items": [
       {
         "uri": "at://did:plc:alice123/community.atproto.resource/1h9tm2hcdz92j",
         "addedAt": "2026-08-15T09:45:20.123Z",
         "note": "Skyline was one of the most popular early clients and pioneered AI-powered content discovery."
       },
       // other early clients...
     ],
     "creator": "did:plc:grace890",
     "tags": ["history", "clients", "archive"],
     "createdAt": "2026-08-15T09:30:15.123Z"
   }
   ```

   This collection provides historical context and preserves information about important resources even after they're no longer active.

## Implementation Considerations

### Indexing Requirements

To implement this lexicon system effectively, the `atproto.community` service will need to maintain several specialized indexes:

1. **Resource Index**:
   - Primary index of all resources with their canonical URIs
   - Normalized URL mapping for duplicate detection
   - Full-text search capabilities for resource discovery
   - Category and tag-based filtering

2. **Amendment Index**:
   - Tracking of all proposed amendments and their status
   - Vote tallies and automatic threshold detection
   - Conflict detection for concurrent amendments

3. **Admin View Cache**:
   - Fast lookup of the current merged state of resources
   - Revision history tracking
   - Change attribution metadata

4. **User Activity Index**:
   - Track user contributions, votes, and reviews
   - Support reputation systems for trusted curators

### User Experience Recommendations

The frontend implementation should consider these user experience elements:

1. **Transparent Amendment Process**:
   - Clear indication when viewing a merged/amended resource
   - Easy access to amendment history
   - Visual highlighting of changed fields

2. **Duplicate Detection**:
   - Real-time feedback during submission
   - Side-by-side comparison of potential duplicates
   - Simple workflow for creating aliases

3. **Community Engagement**:
   - Voting controls directly on amendment proposals
   - Reputation badges for active curators
   - Activity feeds for recent amendments and updates

4. **Resource Discovery**:
   - Advanced filtering by category, status, and tags
   - Featured collections on the homepage
   - Personalized recommendations based on user activity

### Privacy and Moderation

1. **Abuse Prevention**:
   - Rate limiting on submissions and amendments
   - Trusted curator status for frequent contributors
   - Admin tools for removing spam or inappropriate content

2. **Attribution**:
   - Clear credit to original submitters and creators
   - Attribution for amendment contributors
   - Opt-out options for users who don't want to be listed

3. **Content Guidelines**:
   - Clear policies for acceptable resources
   - Process for reporting policy violations
   - Appeals process for rejected amendments

## Conclusion and Next Steps

The proposed lexicon system for `atproto.community` provides a comprehensive framework for a community-maintained resource directory that respects the constraints and capabilities of the AT Protocol. By leveraging community curation through amendments, voting, and aliases, the system can maintain accurate and up-to-date information despite the inherent limitations of the repository ownership model.

### Next Steps

1. **Technical Implementation**:
   - Develop the backend indexing service
   - Create frontend components for resource submission and browsing
   - Implement voting and amendment mechanisms
   - Build duplicate detection algorithms

2. **Community Building**:
   - Establish initial moderation policies
   - Seed the directory with known resources
   - Engage with tool/resource creators for verification
   - Develop documentation for contributors

3. **Integration with AT Protocol Ecosystem**:
   - Create developer documentation for using the lexicon
   - Build APIs for third-party applications to query the directory
   - Explore possibilities for deeper integration with Bluesky clients

By creating this system on `atproto.community`, you'll establish a valuable service for the AT Protocol ecosystem while demonstrating the flexibility and power of the protocol's lexicon system for building community-maintained applications.

The lexicon design prioritizes extensibility, allowing for future enhancements such as version tracking, compatibility indicators, and integration with other AT Protocol services. This foundation will enable the resource directory to evolve alongside the broader ecosystem, providing lasting value to developers and users alike.
