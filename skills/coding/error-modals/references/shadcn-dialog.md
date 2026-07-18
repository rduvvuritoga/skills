# React and shadcn/ui Error Dialog

Use this reference only when the project already uses React and shadcn/ui. Prefer existing project primitives and conventions when they differ.

Require the caller to supply verified copy and executable actions; a generic title or inert primary button hides incomplete product behavior.

```tsx
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
} from "@/components/ui/dialog";
import { Button } from "@/components/ui/button";
import { AlertCircle } from "lucide-react";

type DialogAction = {
  label: string;
  onClick: () => void;
  variant?: "default" | "destructive" | "outline";
};

type ErrorDialogProps = {
  open: boolean;
  onOpenChange: (open: boolean) => void;
  title: string;
  description: string;
  primaryAction: DialogAction;
  secondaryAction?: DialogAction;
};

export function ErrorDialog({
  open,
  onOpenChange,
  title,
  description,
  primaryAction,
  secondaryAction,
}: ErrorDialogProps) {
  return (
    <Dialog open={open} onOpenChange={onOpenChange}>
      <DialogContent className="sm:max-w-md">
        <DialogHeader>
          <div className="flex items-center gap-3">
            <AlertCircle aria-hidden="true" className="h-5 w-5 shrink-0 text-destructive" />
            <DialogTitle>{title}</DialogTitle>
          </div>
          <DialogDescription className="pl-8 pt-1">
            {description}
          </DialogDescription>
        </DialogHeader>
        <DialogFooter>
          {secondaryAction && (
            <Button
              variant={secondaryAction.variant ?? "outline"}
              onClick={secondaryAction.onClick}
            >
              {secondaryAction.label}
            </Button>
          )}
          <Button
            variant={primaryAction.variant ?? "default"}
            onClick={primaryAction.onClick}
          >
            {primaryAction.label}
          </Button>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  );
}
```

Use a destructive primary variant only for a destructive confirmation. For recoverable failures, use the normal primary treatment and reserve red for the error indicator.
