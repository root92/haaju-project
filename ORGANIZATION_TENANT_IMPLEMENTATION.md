# Organization Tenant Implementation

## Overview

This implementation adds organization tenant functionality to the Haaju platform, enabling multi-tenancy support for both personal and business subscriptions. The system now supports organizations that can have multiple users and projects, with proper data isolation and access control.

## Key Features Implemented

### 1. Organization Model (`haaju_backend/users/models.py`)

- **Organization**: Represents an organization/tenant with support for different types (personal, business, NGO, government)
- **OrganizationMember**: Links users to organizations with specific roles (owner, admin, member, viewer)
- **User Model Updates**: Added `default_organization` field and organization-related properties

### 2. Database Schema Changes

#### New Models:
- `Organization`: Core organization entity
- `OrganizationMember`: Many-to-many relationship between users and organizations
- Updated `Project`: Added `organization` foreign key
- Updated `Subscription`: Supports both user and organization subscriptions

#### Key Fields:
- Organization types: personal, business, NGO, government, other
- Member roles: owner, admin, member, viewer
- Contact information: email, phone, website, address
- Organization settings and preferences

### 3. Data Isolation & Security

#### Middleware (`haaju_backend/graphql/middleware.py`):
- **CustomJWTMiddleware**: Enhanced to handle organization context from headers
- **OrganizationMiddleware**: Enforces organization-based data isolation
- Organization context set via `X-Organization` header

#### Access Control:
- Users can only access organizations they're members of
- Projects are filtered by organization context
- Subscription checks work at both user and organization levels

### 4. GraphQL API

#### New Queries:
- `myOrganizations`: Get user's organizations
- `organization`: Get organization by ID
- `organizationBySlug`: Get organization by slug
- `organizationMembers`: Get organization members

#### New Mutations:
- `createOrganization`: Create new organization
- `updateOrganization`: Update organization details
- `addOrganizationMember`: Add member to organization
- `removeOrganizationMember`: Remove member from organization

### 5. Frontend Implementation

#### New Components:
- **Organizations Page** (`haaju_frontend/src/app/organizations/page.tsx`): Organization management interface
- **Organization Hooks** (`haaju_frontend/src/_api/organizationHooks.ts`): React hooks for organization operations
- **GraphQL Types** (`haaju_frontend/src/types/graphql.ts`): TypeScript interfaces for organizations

#### Features:
- Create and manage organizations
- View organization details and statistics
- Manage organization members
- Organization type-specific styling and icons

### 6. Subscription Integration

#### Updated Subscription Model:
- Supports both user and organization subscriptions
- Validation ensures either user OR organization is specified, not both
- Organization subscriptions inherit from organization context

#### Subscription Plans:
- Personal plans for individual users
- Business plans for organizations (NGOs, companies, etc.)
- Feature limits and access control based on subscription tier

## Database Migrations

### Applied Migrations:
1. **users.0006**: Create Organization and OrganizationMember models
2. **users.0007**: Create default personal organizations for existing users
3. **haaju.0017**: Add organization field to Project model
4. **haaju.0018**: Assign existing projects to user's default organizations
5. **haaju.0019**: Make organization field nullable for migration compatibility
6. **subscription.0008**: Add organization support to Subscription model

### Data Migration Strategy:
- Existing users get personal organizations created automatically
- Existing projects are assigned to user's default organization
- Maintains backward compatibility during migration

## Security & Data Isolation

### Multi-Tenancy Implementation:
1. **Organization Context**: Set via HTTP header `X-Organization`
2. **Data Filtering**: All queries filter by organization context
3. **Access Control**: Users can only access organizations they're members of
4. **Role-Based Permissions**: Different roles have different access levels

### Middleware Chain:
1. Authentication middleware sets user context
2. Organization middleware sets organization context
3. GraphQL resolvers filter data by organization
4. Access control checks user permissions within organization

## Usage Examples

### Setting Organization Context:
```javascript
// Frontend API calls with organization context
const headers = {
  'Authorization': `Bearer ${token}`,
  'X-Organization': 'organization-slug-or-id'
};
```

### Creating an Organization:
```javascript
const { create } = useCreateOrganization();
const organization = await create({
  name: 'My NGO',
  slug: 'my-ngo',
  orgType: 'ngo',
  description: 'Non-profit organization'
});
```

### Managing Members:
```javascript
const { add } = useAddOrganizationMember();
await add({
  organizationId: 'org-id',
  userEmail: 'member@example.com',
  role: 'member'
});
```

## Benefits

### For Users:
- **Personal Organizations**: Individual users can manage their personal projects
- **Business Organizations**: Teams can collaborate on shared projects
- **NGO Support**: Non-profits can manage multiple projects and members
- **Data Isolation**: Secure separation between different organizations

### For Platform:
- **Scalability**: Support for multiple organization types
- **Flexibility**: Different subscription models for different use cases
- **Security**: Proper data isolation prevents cross-organization data leaks
- **Growth**: Enables B2B and enterprise features

## Future Enhancements

### Planned Features:
1. **Organization Templates**: Pre-configured settings for different organization types
2. **Advanced Permissions**: Granular permission system within organizations
3. **Organization Analytics**: Cross-project reporting and insights
4. **Bulk Operations**: Manage multiple projects and members efficiently
5. **API Rate Limiting**: Organization-based API usage limits
6. **Custom Branding**: Organization-specific theming and branding

### Technical Improvements:
1. **Caching**: Organization-level caching for better performance
2. **Audit Logging**: Track organization-level changes and access
3. **Backup & Recovery**: Organization-specific data backup strategies
4. **Multi-Region**: Support for organization data in different regions

## Configuration

### Environment Variables:
```bash
# Organization settings
ORGANIZATION_DEFAULT_TYPE=personal
ORGANIZATION_MAX_MEMBERS=100
ORGANIZATION_MAX_PROJECTS=50
```

### Settings Updates:
- Added organization middleware to Django settings
- Updated CORS headers to allow organization header
- Enhanced GraphQL schema with organization types

## Testing

### Test Scenarios:
1. **Organization Creation**: Verify organizations are created with correct defaults
2. **Member Management**: Test adding/removing members with different roles
3. **Data Isolation**: Ensure users can't access other organizations' data
4. **Subscription Integration**: Test organization-level subscription features
5. **Migration**: Verify existing data is properly migrated

### Test Commands:
```bash
# Run organization-specific tests
python manage.py test users.tests.test_organizations
python manage.py test haaju.tests.test_organization_projects
```

## Deployment Notes

### Migration Strategy:
1. Deploy backend changes first
2. Run migrations in order
3. Verify data migration success
4. Deploy frontend changes
5. Test organization functionality

### Rollback Plan:
- All migrations are reversible
- Data migration includes reverse operations
- Frontend changes are backward compatible

## Conclusion

This implementation provides a solid foundation for multi-tenant organization support in the Haaju platform. The system is designed to be scalable, secure, and flexible, supporting both personal and business use cases while maintaining data isolation and proper access control.

The implementation follows best practices for multi-tenancy, includes comprehensive data migration strategies, and provides a clean API for frontend integration. The modular design allows for future enhancements and extensions as the platform grows.
