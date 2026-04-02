## Conditional types
`type NewType<T> = T extends { id: number } ? number : string;

type A = NewType<1>;


type EmailRecipient = {
    name: string;
    email: string;
}

type Email = {
    message: string;
    encoding?: "utf8";
    recipient?: EmailRecipient;
}

type LetterRecepient = {
    name: string;
    country: string;
    city: string;
    zip: number;
    street: string;
}

type Letter = {
    message: string;
    weight: number;
    recipient?: LetterRecepient;
}

let email: Email = {
    message: "New email"
}

let emailRecipient: RecipientOf<Email> = {
    name: "Noah Meyr",
    email: "n.meyer@example.com"
}

let letter: Letter = {
    message: "New letter",
    weight: 15
};

let letterRecepient: RecipientOf<Letter> = {
    name: "Alice Lareson",
    country: "Norway",
    city: "Bergen",
    zip: 121201,
    street: "Hansens vei 586"
}

type AllTypes = Email | EmailRecipient | Letter | LetterRecepient;
type WithName<T> = T extends { name: string } ? T : never;
type Recipient = WithName<AllTypes>;
type WithRecipient<T> = T extends { recipient?: Recipient } ? T : never;
type RecipientOf<T> = T extends Email ? EmailRecipient : T extends Letter ? LetterRecepient : never;
type Message = WithRecipient<AllTypes>;




function sendRecipient<T extends Message>(message: T, receipt: RecipientOf<T>) {
    message.recipient = receipt;
}


sendRecipient(email, emailRecipient);
sendRecipient(letter, letterRecepient)
`
