## Summary
<!-- Provide a 2-4 sentence summary of what changes are introduced and the motivation within the Plataforma Cafetera project. -->

## Linked Issues
- Fixes #
- Refs #

## Challenge Epics Addressed
- [ ] Epica 1: Base de la plataforma y Landing Zone
- [ ] Epica 2: Red, Firewall y Centros de Acopio (Cauca, Huila, Tolima)
- [ ] Epica 3: Ingesta de cultivo, telemetria y buffer local de 24h
- [ ] Epica 4: Servicio de drones, ordenes de fumigacion y misiones
- [ ] Epica 5: Saco, viaje, puerto Buenaventura y consulta publica
- [ ] Epica 6: Infraestructura como codigo (IaC, maximo 100 horas / 2 semanas)
- [ ] Epica 7: Costos y sistema de diseno

## Type of Change
- [ ] New feature (`feat`)
- [ ] Bug fix (`fix`)
- [ ] Documentation update (`docs`)
- [ ] Architecture / Diagram update (`diagram`)
- [ ] Infrastructure as Code (`iac` / `chore`)
- [ ] CI/CD pipeline changes (`ci`)

## Detailed Changes
<!-- Bullet point list of key files and functionalities modified -->
- Added ...
- Updated ...

## Verification & Testing
<!-- How were these changes verified? -->
- [ ] Pytest suite executed successfully for automation scripts
- [ ] Draw.io XML lint passed via diagram_agent_helper.py
- [ ] Documentation and zero-emoji policy verified via docs_helper.py
- [ ] Git commit messages and branch validated via git_helper.py
- [ ] Telemetry / Network CIDR non-overlap verified
- [ ] Privacy constraints verified (no caficultor cedula exposed to public endpoints)
- [ ] Infracost FinOps monthly impact reviewed

## Release Window & Change Governance
- [ ] Complies with harvest freeze rule (No non-emergency changes between 05:00 and 18:00 COT)
- [ ] Rollback steps documented

## Checklist
- [ ] No emojis used across code, comments, documentation, or commit messages
- [ ] Manual push policy observed (changes reviewed before pushing)
- [ ] No secrets, credentials, or private keys committed
- [ ] Documentation matches current project layout
