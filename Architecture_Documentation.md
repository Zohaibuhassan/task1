# Architecture Documentation

## A. Executive Summary
The Space Fractions system is a web-based, interactive learning tool designed to improve fraction-solving skills for sixth-grade students. The system consists of an introductory movie, a main menu, a series of fraction questions, and an ending scene with feedback. The architecture will follow a microservices approach, with separate components for the game logic, question management, and user interface. The system will be deployed on a cloud-based infrastructure, ensuring scalability and availability.

Chosen architectural style: Microservices
Deployment topology: Cloud-based infrastructure

## B. Traceability & Rationale
Requirement ID,Short Text,Diagram(s),Component(s),Artifact filename(s),Rationale
FR-1,Play game,UseCaseDiagram,GameComponent,openapi.yaml,Allows users to play the game
NFR-1,Performance,SequenceDiagram1,GameComponent,internal.proto,Ensures the game responds quickly to user input
ASR-1,Data durability,DeploymentDiagram,QuestionComponent,sql/question_ddl.sql,Ensures that question data is persisted and recoverable

## C. Architecture Overview
The Space Fractions system consists of the following components:
- GameComponent: responsible for game logic and user interaction
- QuestionComponent: responsible for question management and data persistence
- UserComponent: responsible for user authentication and authorization

## D. Detailed Technical Design
### GameComponent
**Recommended default stack**
- Node.js 18
- Express.js 4
- PostgreSQL 14
- Redis 6
- RabbitMQ 3

### Interface design
**External APIs**
```yaml
openapi: 3.0.0
info:
  title: Space Fractions API
  description: API for the Space Fractions game
  version: 1.0.0
paths:
  /play:
    get:
      summary: Play the game
      responses:
        200:
          description: Game started
          content:
            application/json:
              schema:
                type: object
                properties:
                  gameId:
                    type: integer
                    description: Game ID
```

**Internal contracts**
```protobuf
syntax = "proto3";
package spacefractions;
service GameService {
  rpc Play(PlayRequest) returns (PlayResponse) {}
}
message PlayRequest {
  int32 gameId = 1;
}
message PlayResponse {
  int32 gameId = 1;
}
```

### Data model / schema
```sql
CREATE TABLE games (
  id SERIAL PRIMARY KEY,
  game_state JSONB NOT NULL
);
```

## E. Operations & Deployment
### Kubernetes-ready plan
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: spacefractions
spec:
  replicas: 3
  selector:
    matchLabels:
      app: spacefractions
  template:
    metadata:
      labels:
        app: spacefractions
    spec:
      containers:
      - name: spacefractions
        image: spacefractions:latest
        ports:
        - containerPort: 80
```
