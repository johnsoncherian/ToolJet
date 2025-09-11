# ToolJet Documentation

Welcome to the ToolJet project documentation! This directory contains comprehensive documentation for developers, contributors, and users of the ToolJet open-source low-code framework.

## 📚 Documentation Structure

### Core Documentation

- **[Architecture Overview](architecture.md)** - System design, components, and technical architecture
- **[Developer Onboarding](onboarding.md)** - Getting started guide for new contributors
- **[Data Model](data-model.md)** - Database schema, entities, and relationships
- **[Deployment Guide](deployment.md)** - Production deployment strategies and configurations

### Architecture Decision Records (ADRs)

- **[ADR Index](adr/README.md)** - Overview of all architectural decisions
- **[ADR-0001: Record Architecture Decisions](adr/0001-record-architecture-decisions.md)** - Establishing ADR process

## 🚀 Quick Start

### For Developers
1. Read the [Developer Onboarding Guide](onboarding.md)
2. Review the [Architecture Overview](architecture.md)
3. Set up your local development environment
4. Explore the codebase structure

### For Contributors
1. Check the [Contributing Guidelines](../CONTRIBUTING.md)
2. Review existing [GitHub Issues](https://github.com/ToolJet/ToolJet/issues)
3. Join the [Slack Community](https://tooljet.ai/slack)
4. Start with small contributions

### For Users
1. Visit the [Official Documentation](https://docs.tooljet.ai)
2. Try the [Quick Start Guide](https://docs.tooljet.ai/docs/)
3. Explore [Component Reference](https://docs.tooljet.ai/docs/widgets/)
4. Check [Data Source Integrations](https://docs.tooljet.ai/docs/data-sources/)

## 🏗️ Project Architecture

ToolJet is built with a modern, scalable architecture:

- **Backend**: NestJS (Node.js/TypeScript)
- **Frontend**: React 18 with modern tooling
- **Database**: PostgreSQL with TypeORM
- **Real-time**: WebSocket-based collaboration
- **Deployment**: Docker, Kubernetes, Cloud platforms

## 📖 Documentation Standards

### Writing Guidelines
- Use clear, concise language
- Include code examples where helpful
- Keep documentation up-to-date with code changes
- Follow markdown best practices

### Maintenance
- Documentation is reviewed during code reviews
- Major changes require documentation updates
- Regular documentation audits and updates

## 🔧 Development Workflow

### Documentation Updates
1. Make changes to relevant documentation files
2. Test any code examples or procedures
3. Submit pull request with documentation changes
4. Include documentation updates in feature PRs

### ADR Process
1. Create ADR for significant architectural decisions
2. Follow the ADR template and numbering system
3. Update ADR index when creating new records
4. Review and approve ADRs through team consensus

## 📋 Available Commands

The ToolJet project supports several documentation-related commands:

- `/init-docs` - Initialize project documentation structure
- `/update-docs` - Update documentation to reflect code changes
- `/adr "Title"` - Create new Architecture Decision Record
- `/gen-er-diagram` - Generate ER diagram from database models
- `/gen-arch-diagram` - Generate system architecture diagram
- `/check-docs` - Validate documentation freshness
- `/sync-docs` - Sync all documentation with current codebase

## 🤝 Contributing to Documentation

### How to Contribute
1. **Identify gaps**: Look for missing or outdated documentation
2. **Create issues**: Report documentation problems or suggestions
3. **Submit PRs**: Contribute improvements and new documentation
4. **Review**: Help review documentation changes from others

### Areas Needing Documentation
- API reference documentation
- Plugin development guides
- Advanced configuration options
- Performance optimization guides
- Security best practices

## 📞 Support and Community

### Getting Help
- **Documentation Issues**: Create GitHub issues for documentation problems
- **Technical Questions**: Join [Slack Community](https://tooljet.ai/slack)
- **Bug Reports**: Use [GitHub Issues](https://github.com/ToolJet/ToolJet/issues)
- **Feature Requests**: Submit via [GitHub Discussions](https://github.com/ToolJet/ToolJet/discussions)

### Community Resources
- [GitHub Repository](https://github.com/ToolJet/ToolJet)
- [Official Website](https://tooljet.ai)
- [Documentation Site](https://docs.tooljet.ai)
- [Slack Community](https://tooljet.ai/slack)
- [Discord Server](https://discord.gg/tooljet)

## 📄 License

ToolJet is released under the GNU Affero General Public License v3.0. See [LICENSE](../LICENSE) for details.

---

**Note**: This documentation is maintained alongside the ToolJet codebase. For the most up-to-date information, always refer to the latest version in the repository.