import express, { Request, Response } from "express";

const app = express();
app.use(express.json());

interface Task {
    id: number;
    title: string;
    done: boolean;
}

let tasks: Task[] = [];
let nextId = 1;

app.post("/tasks", (req: Request, res: Response) => {
    const { title } = req.body;
    const task: Task = { id: nextId++, title, done: false };
    tasks.push(task);
    res.status(201).json(task);
});

app.get("/tasks", (req: Request, res: Response) => {
    res.json(tasks);
});

app.put("/tasks/:id", (req: Request, res: Response) => {
    const id = parseInt(req.params.id);
    const task = tasks.find(t => t.id === id);
    if (!task) return res.status(404).send("Task not found");
    task.done = req.body.done ?? task.done;
    res.json(task);
});

app.delete("/tasks/:id", (req: Request, res: Response) => {
    const id = parseInt(req.params.id);
    tasks = tasks.filter(t => t.id !== id);
    res.status(204).send();
});

const PORT = 4000;
app.listen(PORT, () => {
    console.log(`Task Manager API running on http://localhost:${PORT}`);
});
