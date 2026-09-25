# Architecture View

1. UseCase — Scenario View: Use Case Diagram
@startuml UseCaseDiagram
left to right direction
actor EndUser as "End User"
actor Admin as "Admin"
rectangle System {
usecase "Play Game" as (PlayGame)
usecase "View Score" as (ViewScore)
usecase "Update Questions" as (UpdateQuestions)
usecase "View Help" as (ViewHelp)
}
EndUser -- (PlayGame)
EndUser -- (ViewScore)
EndUser -- (ViewHelp)
Admin -- (UpdateQuestions)
@enduml
